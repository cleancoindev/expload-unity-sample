This is a simple app that uses the Unity implementation of [Pravda DApp API](https://developers.expload.com/documentation/pravda/integration/dapp-api/) for Expload Desktop. 

**Important note:**
The generated code uses [Json .NET](https://www.newtonsoft.com/json) library for hadling Json.
You can download its Unity version from [AssetStore](https://assetstore.unity.com/packages/tools/input-management/json-net-for-unity-11347).

## Installation

 1. Clone this repo 
 2. Open `ProgramTest` as Unity Project
 3. Select `SampleScene`
 3. Select ‘Send Button’ in Canvas ![SendElement](https://raw.githubusercontent.com/expload/expload-unity-sample/master/pics/SendElement.png)
 4. Select `GUI.cs` as Script for this button ![SendScript](https://raw.githubusercontent.com/expload/expload-unity-sample/master/pics/SendScript.png)

## Repository Structure 

`Assets` contains C# sources including:
 - [`ExploadUnityCodegen.cs`](https://github.com/expload/expload-unity-sample/blob/master/ProgramTest/Assets/ExploadUnityCodegen.cs) is an auxiliary file for generated `Program.cs` that forms requests and parses responses of DApp API.  
 - [`Program.cs`](https://github.com/expload/expload-unity-sample/blob/master/ProgramTest/Assets/Program.cs) is generated file that exposes the functionality to call program methods and parse the raw results.  
 - [`GUI.cs`](https://github.com/expload/expload-unity-sample/blob/master/ProgramTest/Assets/Scenes/GUI.cs) operates with Unity graphics and UI and uses the functionality from `Program.cs`. 

`Program.cs` was automatically generated from another C# source file by [Pravda Dotnet translator](https://developers.expload.com/documentation/pravda/using-dotnet/classes-translation/) and [Pravda code generatation for unity](https://developers.expload.com/documentation/pravda/integration/codegen/).

### Dotnet Translation 

[`SmartProgram.cs`](https://github.com/expload/pravda/blob/master/dotnet-tests/resources/SmartProgram.cs) is a C# source file that is compiled to `SmartProgram.exe` and translated to Pravda program `SmartProgram.pravda` by the following commands: 

```
csc SmartProgram.cs -reference:Pravda.dll
pravda compile dotnet -i SmartProgram.exe -o SmartProgram.pravda
``` 

You can find `Pravda.dll` [here](https://github.com/expload/pravda/blob/master/PravdaDotNet/Pravda.dll).

`Program.cs` and `ExploadUnityCodegen.cs` were generated from `SmartProgram.pravda`:

```
pravda gen unity -i SmartProgram.pravda
```

This command will create an `Assets` folder and place all the generated files into it. 

### SmartProgram

`SmartProgram.cs` contains three methods: 
 - `Emit` that issues tokens to the sender's balance.
 - `BalanceOf` that checks the balance of the given address.
 - `Transfer` that can transfer coins to other address.

`pravda gen unity` analyses these methods and generates the appropriate classes (`EmitRequest`, `BalanceOfRequest`, `TransferRequest`) in `Program.cs`. 
The detailed description of how these classes are generated is available [here](https://developers.expload.com/documentation/pravda/integration/codegen/).

## GUI

This Unity app has a very simple GUI where you should specify the program’s address in the blockchain, what method to run and the arguments for the chosen method. 

It only contains `SampleScene` that is shown in the following picture:

![MainScreen](https://raw.githubusercontent.com/expload/expload-unity-sample/master/pics/MainScreen.png)

