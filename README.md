# CCR for SAS<sup>&reg;</sup>

---
If you use this software, please cite [this article](https://github.com/ejgreene/ccr-sas/blob/master/E%20Greene%20CCR%20paper.pdf):
> Greene, E.J.  A SAS macro for covariate-constrained randomization of general cluster-randomized and unstratified designs. _Journal of Statistical Software_ 2017, 77(CS1):1-20.
---

This SAS macro for covariate-constrained randomization is released under the GNU Lesser General Public License (see `lgpl.txt` and `gpl.txt`).

The sample output in `ccr_example_output.pdf` can be generated from the macro and the dataset `ccr_example_data.sas7bdat` with the command

```SAS
%ccr(ccr_example_data, sid, gid,
     lev1 lev2 lev3 covar,
     d    d    d    c,
     s1   s1   s1   mf.15,
     s1   s1   s1   m200,
     seed=22571);
```

This usage information is lifted from the macro comments; for a more user-friendly article, see [here](https://github.com/ejgreene/ccr-sas/blob/master/E%20Greene%20CCR%20paper.pdf).

```SAS
/****************************/
/* ARGUMENTS AND PARAMETERS */
/****************************/

/* REQUIRED ARGUMENTS:
   dataset = the input dataset, which should have one observation per cluster
   stratid = the variable in the dataset identifying which stratum a cluster
             is in; can leave blank for an unstratified design
   clustid = the variable in the dataset identifying each cluster
   x       = list of covariate variables
   vtype   = 'd' -> dummy covariate
                    (display checks as frequency tables of arms & differences)
             'c' -> continuous/many-valued covariate
                    (display checks as mean/min/q1/median/q3/max of difference)
   slimit  = stratum balancing criteria; set to any for unstratified design
   olimit  = overall balancing criteria
             formatting for criteria: {type}{difference} -or- ANY
             ANY imposes no constraint (allows for constraining only at stratum
               or overall level)
             {type} = 'm' for mean or 's' for sum
             if {difference} starts with f, it's treated as an allowable
               fraction of the total
             FOR EXAMPLE:
             s100 -> sums must differ by no more than 100
             mf.5 -> means must differ by no more than half the overall mean
             NOTE: values in vtype, slimit, and olimit should be separated by
                   whitespace (CCR_V1.0 used asterisks)

   OPTIONAL PARAMETERS:
   arms    = the number of arms to allocate to [default: 2]
   armsize = the variables in the dataset identifying how many clusters in a
               stratum to allocate to each arm
             if not set, clusters are split as evenly as possible between arms
               (at both levels)
   ssample = number of possible stratum allocations to sample and check in
               each stratum [default: 100,000]
   osample = number of combinations of acceptable stratum allocations to
               sample and check [default: 100,000]
             if not set, all combinations will be generated and checked
   seed    = seed for sampling allocations and choosing final randomization
             if not set, SAS will seed from the system clock
               (rand function and proc surveyselect default behavior)
   select  = # of final randomizations to display [default: 1]
   samearmhi = percentage at which clusters appear in the same arm "a lot"
                 [default: 75]
   samearmlo = percentage at which clusters appear in the same arm "rarely"
                 [default: undefined -> 100/(2*arms), which is half chance]
   verbose controls how much is sent to output and log
           (though everything is available afterwards in temporary datasets
            regardless of setting)
      0/false -> only print summaries and checks
      1/true -> print lists of allocations that satisfy the constraints,
                plus the final randomization
      2 -> print lists of all possible within-stratum allocations
      3 -> print lists of all possible (or all sampled) overall allocations
   setting binomsig prints all pairs of clusters whose rates of appearance in
     the same arm differ significantly from chance (parameter sets alpha)
   setting debug prints a lot of macro variables to the log
   setting sizecheck to 0 disables checking whether a sample size
     is larger than the number of overall allocations
*/
```
