# ViewModel Manager

By default uFrame keeps up with viewmodels for us. It maintains a manager for each type of viewmodel you create.

To access these managers, you can inject them into controllers and services using the following code.

```
[Inject] public IViewModelManager<MyViewModel> MyViewModelsManager { get; set; }
```

By default every element controller already generates manager properties just like the above example for the associated element's viewmodel.
