root->plan_params -  PlannerParamItem list; has paramId and Node *item (Var, PlaceHolderVar, or Aggref). var->varno is a member of relids

Param - has paramid but no relid

path->param_info - ParamPathInfo has ppi_req_outer


----


rename `is_parallel_safe` to `should_consider_parallel_for_rel` ?

Do we actually need the split we had at the beginning of the patch series where we intentially track which rel is poisoned?
Or can we simply have the flag on the rel no longer be an absolute and then disallow adding a gather when have a rel that isn't parallel safe yet?
Call out the explicit tradeoff that, absent keeping around a params/no params partial path we may no longer generate parallel plans...but, is this actually true? We previously would have been poisoning things entirely, so that wouldn't result in any fewer parallel plans, but on the other hand: is it possible that we might keep around a cheaper partial path with params that we ultimately can't end up using (and is this why we needed the null guard in `add_partial_path`?
