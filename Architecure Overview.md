RTK is not a new library or a new framework, it is just a set of tools which helps us skip the boilerplate for interacting with Redux for us. Internally, it is generating the same boilerplate (dispatch, actions, settings and unsetting loaders and errors etc), so there is 100% interoperability between the old and new. 

**(1) Making an API Slice and defining a baseQuery**
We use **createAPI** to make an API Slice. This slice ultimately is used to call and store data from APIs.

createAPI  [APIRef-createAPI](APIRef-createAPI.md)

```
 
export const apiSlice = createAPI({
  reducerPath: 'apiSlice',
  baseQuery: fetchBaseQuery({ baseUrl: '/' }),
  endpoints: () => ({})
})
```

- reducerPath: name used for the slice. To access the reducer from the global state directly, we use it. 
- endpoints: The different endpoints associated with the slice. 
- baseQuery: This is the function through which all api calls pass. This abstracts away the finer details of HTTP call making, loader state mgmt etc. Technically, we can create our own baseQuery function, but that would be cumbersome, so we use the defualt fetchBaseQuery helper function to generate the function for us, which can be customized in multiple ways. 


**(2) Code splitting**
RTK recommends having only **one** API slice per session (why? :https://redux-toolkit.js.org/rtk-query/api/createApi) . But, defining all endpoints in one file in one place would be cumbersome, so, we can extend the api slice in different files

Example: 

in a new file reportsAPI.js we import apiSlice, and use **injectEndpoints** to add endpoints 

```
export const reportsApi = apiSlice.injectEndpoints({
.....
})
```


**(3) Adding to store**
Now, in the store, we need to add apiSlice to the other reducers

export const store = configureStore({
  reducer: { ...rootReducer, [apiSlice.reducerPath]: apiSlice.reducer },
})
	

This is the setup required to initialize apiSlice, split endpoints across files and add it to the reducer states. 
