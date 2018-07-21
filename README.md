# Sample of Unity app on Expload platform

This is a simple app implemented on Unity that using Unity implementaion of [Pravda DApp API](https://github.com/expload/pravda/blob/master/doc/dapp-api.md) for Expload client. 

## Structure of repository 
`Assets`, `Packages`, `ProjectSettings` folders contains Unity sources. 
`Assets` contains C# sources including:
 - [`Program.cs`](Assets/Scenes/Program.cs) that sends requests to local Expload client
 - [`GUI.cs`](Assets/Scenes/GUI.cs) that operates with Unity graphics and UI and run requests from `Program.cs`. 

`Program.cs` was automatically generated from other C# source file by [Pravda Dotnet translator](https://github.com/expload/pravda/blob/master/doc/dotnet.md) and [Pravda code generatation for unity](https://github.com/expload/pravda/blob/master/doc/codegen.md). All intermediate files are located in `Aux` folder. 

### Dotnet translation 
[`SmartProgram.cs`](Aux/SmartProgram.cs) is C# source file that is compiled ([`SmartProgram.exe`](Aux/SmartProgram.exe)) and translated to Pravda program ([`SmartProgram.pravda`](Aux/SmartProgram.pravda)) by the following commands: 
```
csc SmartProgram.cs -reference:expload.dll 
pravda compile dotnet -i SmartProgram.exe -o SmartProgram.pravda
``` 

`Program.cs` was generated from `SmartProgram.pravda`:
```
pravda gen unity -i SmartProgram.pravda -d .
```

## SmartProgram
`SmartProgram.cs` contains three methods: 
 - `emit` that emits tokens to the sender's balance;
 - `balanceOf` that checks balance of the given address;
 - `transfer` that can transfer coins to other address.

Based on them there were generated 3 classes in `Program.cs`. All of them have a very similar structure, so let's take a look only at one of them -- `BalanceOfRequest`:
```
class BalanceOfRequest: ProgramRequest<int> {

        public BalanceOfRequest(byte[] programAddress) : base(programAddress) { }

        protected override int ParseResult(string json)
        {
            return IntResult.FromJson(json).value;
        }

        public IEnumerator BalanceOf(byte[] arg0)
        {
            String json = String.Format("{{ \"address\": {0}, \"method\": \"balanceOf\", \"args\": [{{ \"value\": {1}, \"tpe\": \"bytes\" }}] }}",  "\"" + BitConverter.ToString(ProgramAddress).Replace("-","") + "\"" ,  "\"" + BitConverter.ToString(arg0).Replace("-","") + "\"" );
            yield return SendJson(json);
        }
}
```
It inherits from `ProgramRequest` class that prepares requests, sends them via Unity http client and parses the received answer. 

Constructor takes address of program in the blockchain. `ParseResult` method parses received json to retrieve balance. `BalanceOf` method generates neccessary json and sends it by `SendJson` method from `ProgramRequest` class.

These classes provide *asynchronous* API via Unity coroutines. 
You can run such coroutine as follows:
```
var req1 = new BalanceOfRequest(address);
yield return req1.BalanceOf(<some_address>);
```
See more examples in [`GUI.cs`](Assets/GUI.cs). 

## GUI
This Unity app has very simple GUI where you should specify address of program in the blockchain, what method to run and specify arguments for chosen method. 


