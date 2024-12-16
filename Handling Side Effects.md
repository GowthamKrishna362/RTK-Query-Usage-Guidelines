**(1) Updating a non-api cache after API call**

We have access to **dispatch** and **getState** arguments in [OnQueryStarted](OnQueryStarted.md), amongst others we can do updates on any reducer- new slice based or old ones
Use getState to access existing values

```
onQueryStarted: async(arg, {dispatch, queryFulfilled, getState })) => {
	try {
		await queryFulfilled
		dispatch(closeModal()) // Old reducer example
		dispatch(reportsSlice.increaseCount()) // New reducer example 		
	} catch(e) {
	
	}
}
```

**(2) Calling another api after one api is complete**

Example, if on saving report, the fetchReports should be invalidated:-

(a) Manually
```
onQueryStarted: async(arg, {dispatch, queryFulfilled, getState })) => {
	try {
		await queryFulfilled
		dispatch(reportsApi.endpoints.fetchReports.initiate(arg))	
	} catch(e) {
	
	}
}
```

(b) Better way: invalidate the tag. Then, wherever useFetchRepoertQuery() hook is used, the API will be automatically called since the cache is invalidated.

```

tagTypes: ["REPORTS_LIST"],
builder.query({
	query: () => ({})
	providesTags: ["REPORTS_LIST"]
}),
builder.mutation({
	query:()  => ({})
	invalidatesTags: ['REPORTS_LIST']
}),
```

**(3) Multiple APIs**

Use cases: Promise.all([,,,,,,]), chaining APIs, choosing what endpoint to hit based on conditions 
Different ways to compose multiple APIs and make it reusable


- (1) in a helper file, use the [Usage without hooks](Usage%20without%20hooks.md) strategy
- (2) Use [queryFn](queryFn.md) to compose
- (3) Probably the best and most thematic: make hooks, and use the appropriate query/mutation hooks