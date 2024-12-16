We could have a folder structure like this per module:

reports
- reportsSlice.js
- reportsApi.js
- reportsSelectors.js

This would be the ideal endpoint, but for the existing code, we could make the folder and add to the API slice one by one. 
The da reducer can serve as the "slice", and with each API that we touch, we could move it one by one into the daApi Slice. Since there is 100% interoperability, we can move bit by bit. 

deepAnalytics
- deepAnalyticsReducer.js
- deepAnalyticsApi.js


The most ideal APIs to easily move are the simple Loader -> API -> Response/Error flows, which don't have any complex side effects or multiple API calls. 


