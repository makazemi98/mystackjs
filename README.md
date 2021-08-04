# mystackjs
stacks - tidy model stacking

stacks is an R package for model stacking that aligns with the tidymodels. Model stacking is an ensembling method that takes the outputs of many models and combines them to generate a new model—referred to as an ensemble in this package—that generates predictions informed by each of its members.

The process goes something like this:

    Define candidate ensemble members using functionality from rsample, parsnip, workflows, recipes, and tune
    Initialize a data_stack object with stacks()
    Iteratively add candidate ensemble members to the data_stack with add_candidates()
    Evaluate how to combine their predictions with blend_predictions()
    Fit candidate ensemble members with non-zero stacking coefficients with fit_members()
    Predict on new data with predict()

You can install the package with the following code:

install.packages("stacks")

Install the development version with:

remotes::install_github("tidymodels/stacks", ref = "main")

stacks is generalized with respect to:

    Model type: Any model type implemented in parsnip or adjacent packages is fair game to add to a stacks model stack. Here’s a table of many of the implemented model types in the tidymodels core, with a link there to an article about implementing your own model classes as well.
    Cross-validation scheme: Any resampling algorithm implemented in rsample or adjacent packages is fair game for resampling data for use in training a model stack.
    Error metric: Any metric function implemented in yardstick or adjacent packages is fair game for evaluating model stacks and their members. That package provides some infrastructure for creating your own metric functions as well!

stacks uses a regularized linear model to combine predictions from ensemble members, though this model type is only one of many possible learning algorithms that could be used to fit a stacked ensemble model. For implementations of additional ensemble learning algorithms, check out h2o and SuperLearner.

Rather than diving right into the implementation, we’ll focus here on how the pieces fit together, conceptually, in building an ensemble with stacks. See the basics vignette for an example of the API in action!
a grammar

At the highest level, ensembles are formed from model definitions. In this package, model definitions are an instance of a minimal workflow, containing a model specification (as defined in the parsnip package) and, optionally, a preprocessor (as defined in the recipes package). Model definitions specify the form of candidate ensemble members.