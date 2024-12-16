Reference: https://redux-toolkit.js.org/rtk-query/usage/prefetching
API Ref: [APIRef- Prefetch](APIRef-%20Prefetch.md)

The goal of prefetching is to make data fetch _before_ the user navigates to a page or attempts to load some known content.

There are a handful of situations that you may want to do this, but some very common use cases are:

1. User hovers over a navigation element
2. User hovers over a list element that is a link
3. User hovers over a next pagination button
4. User navigates to a page and you know that some components down the tree will require said data. This way, you can prevent fetching waterfalls.

the `usePrefetch` hook will not run automatically — it returns a "trigger function" that can be used to initiate the behavior.

Refer [Query Usage](Query%20Usage.md) for usage. 

Customization: 
1. `ifOlderThan` - (default: `false` | `number`) - _number is value in seconds_
    - If specified, it will only run the query if the difference between `new Date()` and the last `fulfilledTimeStamp` is greater than the given value
2. `force`
    - If `force: true`, it will ignore the `ifOlderThan` value if it is set and the query will be run even if it exists in the cache.