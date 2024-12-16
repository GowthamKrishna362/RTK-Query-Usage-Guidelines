Mutations are used to send data updates to the server and apply the changes to the local cache. Mutations can also invalidate cached data and force re-fetches using tags.

```
reportsApi.js:

    saveReportChanges: builder.mutation({
      query: (payload) => ({
        url: REPORT_BASE,
        body: payload,
        method: 'PUT',
      }),
      invalidatesTags
      transformResponse,
      transformErrorResponse,
      async onQueryStarted,
      async onCacheEntryAdded
    })

Component: 
const [updatePost, {data, error, isUninitialized, isLoading, isSuccess, isError}] = useSaveReportChangesMutation()
```


By default, separate instances of a `useMutation` hook are not inherently related to each other. 
Triggering one instance will not affect the result for a separate instance.
This applies regardless of whether the hooks are called within the same component, or different components. 
Use **fixedCacheKey** property to share values like data, error, loading states etc, otherwise, they are independent. 

```
const [updatePost, {data, error,..}] = useUpdatePostMutation({  
	fixedCacheKey: 'shared-update-post',  
})

onClick = () => {
	updatePost(arg).unwrap().then().catch()
}

```