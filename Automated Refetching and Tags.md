**Refer** https://redux-toolkit.js.org/rtk-query/usage/automated-refetching#:~:text=Tags%E2%80%8B&text=For%20RTK%20Query%2C%20tags%20are,be%20affected%20by%20the%20mutation.

RTK Query uses a "cache tag" system to automate re-fetching for query endpoints that have data affected by mutation endpoints. This enables designing your API such that firing a specific mutation will cause a certain query endpoint to consider its cached data _invalid_, and re-fetch the data if there is an active subscription.

3 parts:

**tagTypes**: defined at the slice level , has all the tag types which can be provided.  
**providesTags** - tags associated with a query. Array of String  of tagTypes, or Array of Objects {type: String, id?: number}, or this can be a function which returns those depending on the result
**invalidatesTags** - tags that a mutation invalidates, thereby invalidating the caches of the queries which contain the tags.  Same type as above. 

Example - When we save, or delete, or create a report, we want the api cache for all reports list to be invalidated.Can also make invalidatesTags a function, which takes the result, so only if there is a succesful post, we can update


```
    fetchReports: builder.query({
      query: () => REPORT_BASE,
      providesTags: ['ALL_REPORTS'],
    })

    saveReportCopy: builder.mutation({
      query: ({ payload, reportType }) => ({
      ....
      }),
      invalidatesTags: ['ALL_REPORTS'],
  })
      createReport: builder.mutation({
      query: ({ payload, reportType }) => ({
			....
      }),
      invalidatesTags: ['ALL_REPORTS'],
      })



```

calling saveReportCopy, createReport will invalidate the fetchReportsCache (can be customized to invalidate on success only too)

tags can have multiple levels too

```
{type: "REPORTS", id: "1003"}
```

**Invalidating tags in component**

```
const invalidatePostTags = () => { dispatch(apiSlice.util.invalidateTags(['Post'])); };
```