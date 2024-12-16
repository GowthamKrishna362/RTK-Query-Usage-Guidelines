
Every query and mutation has function which can be passed which is called on queryStart. This can be used for any side effects. 
Note: There is no onQuerySuccess or on QueryEnd, to do it, we need to await the **fulfilled** object.
We have access to many arguments from the function, including **dispatch** and **getState**, which can be used for any side effects, including calling other APIs, and dispatching state updates to other slices, like closing modals
**Note**: 
- transformResponse, transformErrorResponse are called before control is passed to onQueryStarted, so if you have a transformErrorResponse, the **error object** will be transformed before being sent to catch block. Similar to response in try block and transformResponse
- The error object which is passed to the catch block always be of the form {error, meta, isUhandledError}. The one which is transformed is the **error** field

```
getPost: build.query({

	query:
	onQueryStarted: async(arg, {
		dispatch,  
		getState,  
		extra,  
		requestId,  
		queryFulfilled,  
		getCacheEntry,  
		updateCachedData
	}) => {
		... things to do on query start
		try {
			const res = await queryFulfilled
			dispatch(closeModal())	
		} catch(e) {
			e.error
			... things to do on query error
			
		}
	}
})
```

