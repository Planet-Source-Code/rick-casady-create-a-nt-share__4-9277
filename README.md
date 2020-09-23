<div align="center">

## Create a NT Share


</div>

### Description

Create a share in a windows environment without using DOS commands. This code was based on information learned in WMI Scripting Primer: Part 3 on the MSDN<br> This code can easily be converted to VB.NET...
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Rick Casady](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/rick-casady.md)
**Level**          |Intermediate
**User Rating**    |4.0 (12 globes from 3 users)
**Compatibility**  |VbScript \(browser/client side\)

**Category**       |[Security](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/security__4-14.md)
**World**          |[ASP / VbScript](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/asp-vbscript.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/rick-casady-create-a-nt-share__4-9277/archive/master.zip)





### Source Code

```
'<pre>
' Purpose: Create a network share for NT users. You will
' need to have correct permision to do this and it can be
' done to remote computers.
' We pass in the folder name, path of the folder and
' description of the share folder.
Private Sub CreateShare(strShareName, strPath, strDescription)
	Dim objSWbemServices as object  
	Dim objSWbemObject as object    
	Dim colSWbemObject as object  
    Dim intRet as integer  
    Dim blnExists as boolean  
    Dim objSWbem as object
    ' Next we call the standard GetObject function for
    ' returning COM objects and pass it the connection
    ' string for connecting to the WMI.
    set objSWbemServices = GetObject("winmgmts:\\.\root\cimv2")
    ' This same line can be executed on a remote computer
    ' with a differnt connection string like this:
    ' objSWbemServices = GetObject("winmgmts:\\" & strComputer & "\root\cimv2")
   
    ' Now we enumrate the Shares on the target computer and
    ' return it to a collection
    set colSWbemObject = objSWbemServices.InstancesOf("Win32_Share")  
    ' Loop through each share on the machine to see if it already exists
   	For each objSWbem in colSWbemObject  
   		If(objSWbem.name = strShareName)Then  
			blnShareExists = True
			Exit For
		Else           
			blnShareExists = False
	    End If
	Next  
	' if the share didnt exisit our boolean will be false
	' and we can try to add it.
	If (blnShareExists = False)Then
    	' Create the share
    	' Now we need to get
   		set objSWbemObject = objSWbemServices.Get("Win32_Share")  
   		' Last we call the create passing our pathg, name,
   		' description and 10 is for max number of users
    	intRet = objSWbemObject.Create(strPath, strShareName, , 10, strDescription)
    Else
    	msgbox("Folder aready shared")
    End If
End Sub
'</pre>
```

