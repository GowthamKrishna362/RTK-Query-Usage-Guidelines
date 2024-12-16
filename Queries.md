Use queries for requests that retrieve data. For anything that alters data on the server or will possibly invalidate the cache, use mutations.
Typically, they are GET requests, but can be POST or PUT too, if you want to send payloads

**Defining Query Endpoints**

Using builder.query. Full ref - [APIRef - builder.query()](APIRef%20-%20builder.query().md)
Each endpoint can take one argument. There can only be one, if more needed, send as object, this will be serialized for cache key purposes.


```
export const demoApi = apiSlice.injectEndpoints({
  endpoints: (builder) => ({
    getPosts: builder.query({
      query: () => 'posts',
    }),
    getPost: builder.query({
      query: (id) => `posts/${id}`,
    }),
    getPostWithPayload: builder.query({
      query: ({ payload }) => ({
        url: 'posts',
        body: payload,
        param: {
	        search: true
        }
        method: 'POST',
      }),
      transformResponse:
      transformErrorResponse:
      onQueryStarted:
      providesTags:
      onCacheEntryAdded: 
    }),
  }),
});
```

This automatically generates hooks, attached to the demoApi and apiSlice, which can be exported like this

```
export const {useFetchPostsQuery, useLazyFetchPostsQuery...} = demoApi
```


**Using Queries With Hooks**

**(1) useQuery**

Most common way.

```  
// Have to pass undefined as param since there is no param for this query
  const { data, isLoading, error... } = useFetchPostsQuery(queryArg, queryOptions);
```

**Default behaviour:**
- Checks cache. 
	- In brief - uses endpoint + argument as cache query key. If argument is an object, it is serialized for this purpose, and deepCompare is done
- If it is a cache hit, the api will be called automatically, otherwise, data will be sent from the cache, and api is not called
- Cache is cleared when there are no **subscribers** (ie mounted components) after a certain time, by default 60s. This can be overwritten with **keepUnusedDataFor**
- For more, refer [Cache Behaviour](Cache%20Behaviour.md)

Behaviour can be customized with both options at the API level in the slice ([APIRef - builder.query()](APIRef%20-%20builder.query().md) ) and at the hook level with queryOptions object ([APIRef- useQuery](APIRef-%20useQuery.md))

Important queryOptions: (These can be passed both at the hook level and the api level)
**skip** :- Condition to skip making the call
**pollingInterval**:- Refetch at specified interval
**refetchOnMountOrArgChange**: - Refetch everytime remount happens, on top of the arg change condition. This can also be an integer.(**Value is in seconds**) If a number is provided and there is an existing query in the cache, it will compare the current time vs the last fulfilled timestamp, and only refetch if enough time has elapsed.

```
const {data} = useGetPostQuery(id, {refetchOnMountOrArgChange: true})
```

**Note: 

When there are endpoints which have **no args**, and you want to pass options, **you should pass undefined** as the arg, since options is the second parameter. **This is a recurring theme with multiple use cases**

```
const {data} = useGetPostsQuery(undefined, {refetchOnMountOrArgChange: true, skip: bool})
```

**PROGRAMMMATIC WAYS OF CALLING**

**(2) useQuery + refetch**

the useQueryHook exposes "refetch" function

```
const {refetch, data} = useGetPostsQuery(arg, options)
const {refetchPostWithId : refetch}
onClick = () => {
	try {
		const res = await refetch().unwrap()
	} catch(e) {
	
	}

}
```

Refetches the data. Takes no argument, any args should be passed in the hooks as usual

**Note**: To await the data, and get the response immediately, refetch needs to be chained with unwrap(), otherwise it is just called, and the result and errors will be populated in the hook result. If you do unwrap, you should catch errors as they are thrown. **This is a recurring theme with multiple use cases**

**(3) useLazyQuery**

```
const [fetchPosts, {data, error, ...etc}] = useLazyGetPostsQuery(options)
const handleFetch =() => {
	fetchPosts(arg, preferCacheValue = false).unwrap().then().catch()
}
```

- Does not fetch automatically
- Argument passed in function,options passed in the hook (except **preferCacheValue** , which will skip call if cached value is present) 
- awaiting/promise chaining using unwrap()

