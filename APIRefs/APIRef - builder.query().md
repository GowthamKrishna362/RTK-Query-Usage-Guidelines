builder.query()
**Reference:** https://redux-toolkit.js.org/rtk-query/usage/queries

**Important ones** : query, transformResponse, transformErrorResponse, providesTags, onQueryStarted, onCacheEntryAdded, keepUnusedDataFor:

```
build.query({
      query: (id) => ({ url: `post/${id}` }),
      queryFn?:
      transformResponse: (response, meta, arg) => response.data,
      transformErrorResponse: (response, meta, arg) => response.status,
      providesTags: (result, error, id) => [{ type: 'Post', id }],
      async onQueryStarted(
        arg,
        {
          dispatch,
          getState,
          extra,
          requestId,
          queryFulfilled,
          getCacheEntry,
          updateCachedData,
        }
      ) {},
      async onCacheEntryAdded(
        arg,
        {
          dispatch,
          getState,
          extra,
          requestId,
          cacheEntryRemoved,
          cacheDataLoaded,
          getCacheEntry,
          updateCachedData,
        }
      ) {},
      keepUnusedDataFor: 
      refetchOnMountOrArgChange:
      refetchOnFocus:
      refetchOnReconnect:
      extraOptions:
      serializeQueryArgs:
      merge:
      forceRefetch:
```