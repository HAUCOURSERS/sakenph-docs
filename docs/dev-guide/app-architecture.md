# App Architecture

## Top Layer
- Consists of app initialization code and the location of the primary widgets
```mermaid
flowchart TD
    main(["main.dart"])
    main --> inittProviders["Initialize Providers"]
    main --> buildWidget["Build HomePage Widget"]

    buildWidget --> initState["initState()"]

    initState --> getUserLoc["Get user loc and save"]
    initState --> mountToSysTaskProvider["Mount providers to SystemTasksProvider"]
    initState --> initPrimaryWidgets["Initialize primary widgets"]

    initPrimaryWidgets --> MapWidget["MapWidget"]
    initPrimaryWidgets --> BackgroundWidget["BackgroundWidget"]
    initPrimaryWidgets --> ForeGroundWidget["ForegroundWidget"]

```
### Layering Visual
![](/img/layer_visual.jpg)

:::info
It's assembled this way in order to easily manage what widgets to appear based on app state
:::

## Primary Widgets

### Foreground Widget
```mermaid
flowchart TD
    start(["start"]) -->
    providers["SearchDetailsProvider, SystemVariablesProvider"] -->
    listenForVals["Listen for isFromLocationDetailsEmpty, suggestedShortestPaths.isNotEmpty, appCurrentState, backgroundWidgetVisibility"]

    listenForVals -- "systemState value" --> 
    switchCase{"Switch Case<br/>for systemState<br/> value"} --> 
    renderWidgetAccordingly["Render widgets according to code logic"]
    


```

### Background Widget
```mermaid
flowchart TD
    start(["Start"]) -->
    systemVariablesProvider["Get info from SystemVariablesProvider"] -->
    listenForVals["Listen for backgroundWidgetVisibility, appCurrentState, backgroundWidgetColor values"]
    listenForVals -- "backgroundWidgetColor value" --> adjustBackgroundWidgetColor["Adjust the Background Widget's color accordingly"]
    listenForVals -- "backgroundWidgetVisibility value" --> adjustVisibility{"Background<br/>Widget Visibility"}
    adjustVisibility -- "true" --> adjustVisibilityTrue["Background Widget will be visible"]
    adjustVisibility -- "false" --> adjustVisibilityFalse["Background Widget will be invisible. GestureDetector functions will not work"]
    listenForVals -- "appCurrentState value" --> performSwitchCase["Perform Switch Case"]

    performSwitchCase -- "SystemState.value" --> willDisplayWidgetAccordingly["Will display widgets according to the code's logic"] 
```