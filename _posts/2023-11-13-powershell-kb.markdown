---
layout: post
title:  "PowerShell Reference Materials"
categories: powershell
tags:
date: 2023-11-13 11:00:00 +1:00
---

This is a list of materials I found useful while learning PowerShell.

## Docs
1. [PowerShell 101](https://learn.microsoft.com/en-us/powershell/scripting/learn/ps101/00-introduction?view=powershell-7.3)

## Books
1. [PowerShell Deep Dives](https://www.manning.com/books/powershell-deep-dives)

## Snippets

### ShouldProcess; WhatIf; Confirm;
Source: [Everything you wanted to know about ShouldProcess](https://learn.microsoft.com/en-us/powershell/scripting/learn/deep-dives/everything-about-shouldprocess)
```powershell
## Adding support for -WhatIf and -Confirm. Our function will also set those params for all cmdlets it's calling if they support it.
function Test-ShouldProcess {
    [CmdletBinding(SupportsShouldProcess)]
    param()

    $file = Get-ChildItem './myfile1.txt'
    ## Here we're adding support for -WhatIf and -Confirm to our custom code
    if($PSCmdlet.ShouldProcess($file.Name)){
        $file.Delete()
    }

    <## Processing collections
    The reason why I place ShouldProcess tightly around the change,
    is that I want as much code to execute as possible when -WhatIf is specified.
    I want the setup and validation to run if possible so the user gets to see those errors.
    #>
    foreach ($node in $collection){
        # general logic and variable work
        if ($PSCmdlet.ShouldProcess($node,'OPERATION')){
            # Change goes here
        }
    }
}

## Override default messages
### $PSCmdlet.ShouldProcess('TARGET')
What if: Performing the operation "FUNCTION_NAME" on target "TARGET".

### $PSCmdlet.ShouldProcess('TARGET','OPERATION')
What if: Performing the operation "OPERATION" on target "TARGET".

### $PSCmdlet.ShouldProcess('MESSAGE','TARGET','OPERATION')
What if: MESSAGE


```
