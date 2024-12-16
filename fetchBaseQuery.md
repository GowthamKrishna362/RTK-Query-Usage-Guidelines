**Refer** https://redux-toolkit.js.org/rtk-query/api/fetchBaseQuery

It is the function used to generate the function which all API endpoints go through (the baseQuery). We can define some global behaviours here

This function will contain all the logic to invoke the http request, manage state etc. 

**Arguments** [APIRef-fetchBaseQuery](APIRef-fetchBaseQuery.md)

**Practical Example***:- Globally populate headers, so we dont have to pass getPostheaders() or getGetHeaders() everywhere


```
export const apiSlice = createApi({
  baseQuery: fetchBaseQuery({
    baseUrl: '/',
    credentials: 'same-origin',
    prepareHeaders: (headers, { arg, extra, endpoint, type }) => {
      let headersToPopulate;
      switch (arg?.method) {
        case 'PUT':
        case 'POST':
        case 'DELETE':
          headersToPopulate = getPostHeaders().headers;
          break;
        case 'GET':
          headersToPopulate = getGetHeaders().headers;
          break;
        default:
          headersToPopulate = getGetHeaders().headers;
      }
      // Prepare headers is called at the end, so it overrides the headers passed in the different endpoint queries, so doing this 
      Object.entries(headersToPopulate).forEach(([headerKey, headerValue]) => {
        if (!headers.has(headerKey)) {
          headers.set(headerKey, headerValue);
        }
      });
    },
  }),
  reducerPath: 'apiSlice',
  endpoints: () => ({}),
});
```