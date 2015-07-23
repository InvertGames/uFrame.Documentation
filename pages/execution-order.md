# Execution Order

## Views

### For Views instantiated at runtime

Awake > OnEnable > PreBind > Bind > AfterBind > InitializeViewModel > Start > Update loop begins 

### For Views existing "SceneFirst" before runtime

Awake > OnEnable > CreateModel > InitializeViewModel > Start (before base call) > PreBind > Bind > AfterBind > Start (after base call) 

### When Destroying an object

OnDisable > OnDestroy (before base.OnDestroy() call) > UnBind > OnDestroy (after base.OnDestroy() call) 
