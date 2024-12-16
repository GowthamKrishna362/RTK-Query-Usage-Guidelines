

**(1) API call on useEffect**
The equivalent for this would be to  use useQueryHook and pass refetchOnMountOrArgsChange: true, either on the hook level or endpoint level, but this could be done more efficiently with tags.

```
const {data} = useGetPostsQuery(undefined, {refetchOnMountOrArgChange: true})
```

If the useEffect has condition to check if data is present or not the equivalent would be to use the hook without the flag. 

```
const {data} = useGetPostsQuery()
```


**(2) Refetching data**
Can use the refetch() function, but refetching data after some mutation other api or some condition might be better done with **tags**
```
const {refetch, data} = useGetPostsQuery(arg, options)
onRefresh = () => {
	refetch().unwrap().then().catch()
}

```

**(3)** **Calling in onClick etc** 
Use lazyQuery

```
const [fetchPosts, {data, error, ...etc}] = useLazyGetPostsQuery(options)

const handleFetch =() => {
	fetchPosts(arg, preferCacheValue = false).unwrap().then().catch()
}
```

**(4)** **Calls with dependency of one call on another call's results**
Can be done with lazy and useEffect, but better way:- 

```
const {data : result, isLoading} = useFirstQuery()
const {...} = useSecondQuery(data, {skip: isLoading }) 
```

**(5) Polling**
Refer: https://redux-toolkit.js.org/rtk-query/usage/polling
```
const {...} = usePostsQuery(undefined, {pollingInterval: 30})
```

**(6) Prefetch**  
Refer: https://redux-toolkit.js.org/rtk-query/usage/prefetching

Basic:
```
in postsApi.js
export const {useFetchPostQuery, usePrefetch } = postsApi

in component

const prefetchPost = usePrefetch('getPosts', prefetchOptions)

onMouseEnter = () => {
	prefetchPost(postId, prefetchOptions)
}

<button onMouseEnter= {onMouseEnter}/>

```

We can also do more advanced automated prefetching, but dont see a use case for it yet: https://redux-toolkit.js.org/rtk-query/usage/prefetching#automatic-prefetching