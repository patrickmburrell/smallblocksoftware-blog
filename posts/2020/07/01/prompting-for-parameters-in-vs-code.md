<!--
.. title: Prompting for Parameters in VS Code
.. slug: prompting-for-parameters-in-vs-code
.. date: 2020-07-01 12:00:00 UTC-04:00
.. tags: vscode
.. category: software
.. link:
.. description:
.. type: text
-->

While debugging a node.js app in VS Code at work, I thought it sure would be nice if I could pass a parameter into my app to decide what deployment stage to target. A few minutes with Google turned up the info I needed to make this happen.

First, I found how to read parameters from the command line:

```shell
const stage = process.argv[2]
```

Next up: get VS Code to prompt me for that stage when launching the debugger. I found out I needed to modify the `.vscode/launch.json` file in the project folder. Here is a cleaned-up version of that file:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "DevOps: create tables",
      "program": "${workspaceRoot}/devOps/actions/createTables.js",
      "args": ["${input:pickStage}"],
      "cwd": "${workspaceRoot}",
      "outputCapture": "std"
    }
  ],
  "inputs": [
    {
      "id": "pickStage",
      "description": "Select target stage",
      "type": "pickString",
      "options": ["res", "dev", "pro"],
      "default": "res"
    }
  ]
}
```

The new parts I had to add were the `${input:pickStage}` value in the `args` array and the `inputs` array with my stage values (`res`, `dev`, `pro`) and the default choice (`res`).

Holy cats, do I love VS Code!
