
https://github.com/reduxjs/redux-toolkit/blob/master/docs/rtk-query/api/created-api/endpoints.mdx


```
type EndpointLogic = {
  initiate: InitiateRequestThunk
  select: CreateCacheSelectorFactory
  matchPending: Matcher<PendingAction>
  matchFulfilled: Matcher<FulfilledAction>
  matchRejected: Matcher<RejectedAction>
}


initiate signature:
type InitiateRequestThunk = StartQueryActionCreator | StartMutationActionCreator;

type StartQueryActionCreator = (
  arg:any,
  options?: StartQueryActionCreatorOptions
) => ThunkAction<QueryActionCreatorResult, any, any, UnknownAction>;

type StartMutationActionCreator<D extends MutationDefinition<any, any, any, any>> = (
  arg: any
  options?: StartMutationActionCreatorOptions
) => ThunkAction<MutationActionCreatorResult<D>, any, any, UnknownAction>;

type SubscriptionOptions = {
  pollingInterval?: number;
  refetchOnReconnect?: boolean;
  refetchOnFocus?: boolean;
};

interface StartQueryActionCreatorOptions {
  subscribe?: boolean;
  forceRefetch?: boolean | number;
  subscriptionOptions?: SubscriptionOptions;
}

interface StartMutationActionCreatorOptions {
  track?: boolean;
}

```