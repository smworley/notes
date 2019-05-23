
# VBA notes    
By Sarah Worley ([@smworley](https://github.com/smworley) on github :sparkling_heart:).  

Visual Basic for Applications (VBA) is a language behind all windows office applications that allows for a highly customizable excel and application experience. It allows for a user to write macros which are modules consisting of functions and subroutines to define further behavior for excel. I found it particularly useful when working with data sets, even when the file extensions were missing. It's flexible like that. :smile:

Also, highlight.js doesn't support syntax highlighting for VBA, so all my code is grey and boring. Sorry. 

**Purpose of this document:**  
These are personal notes and to help anyone else it might help. This will probably be the most useful to intermediate programmers who are just looking for a fast startup guide to VBA.

I put the tutorials I used to learn this in the bottom so if you want a more details on a particular subject the links are down there and Microsoft provides lists on lists of what you can access of their objects.  

## Table of Contents:  
- [Setting Up Excel for VBA](#setting-up-excel-for-vba)  
- [Variables](#variables)  
- [Functions](#functions)  
- [Subroutines](#subroutines)  
- [Debugging](#debugging)  
- [Excel Shortcuts](#shortcuts)  
- [Credits](#credits)  



## Setting Up Excel for VBA [](#setting-up-excel-for-vba)  
0. Open Excel  
1. Go to Options -> Customize Ribbon  
2. Under the Main Tabs box on the right, check the Developer Box  
3. Click Okay   
Make sure your project is currently open as a .xlsm, or a macro enabled workbook. Create one if it's not.  
4. Switch to the developer tab in the top ribbon  
5. Click on 'Visual basic'  

Viola! The editor which will allow you to create and customize macros will open.  

### Some other things to make life easier:  
**Getting a terminal window :**  
0. View -> View Immediate   
The immediate window is where your Debug.Print statements will output.  

**Making the editor less annoying (because it is profoundly annoying):**  
0. Tools -> Options  
1. In the editor tab, uncheck 'automatic syntax check.' This stops the editor from making a dialog box every time you make a mistake that stops you from typing.  
2. Check the box that says 'require variable declaration.' This will stick a line at the top of your new module that says 'Option Explicit' that just makes life a lil easier by removing ambiguity around variables.

**The holy grail, recording macros:**   
This is what it sounds like. You do an action, and then excel figures out what you did and automatically generates the exact code to do it. It's not pretty code, but it's decent all things considered. HOWEVER it is not the *best* solution. It will always hardcode values to the exact windows you were using, so if you want your macro to work across multiple workbooks/cells, you'll need to replace those values with variables.  
0. Developer ribbon -> record macro   
1. Do some stuff.   
2. When finished, developer ribbon -> stop recording.   
3. Open visual basic to view the code.   
4. Profit.   



## Variables[](#variables)   

```
Dim hello As String			' Mutable value  
Constant helpme As String 	'Immutable Value  
```

= Equals sign sets the value of a variable.  

:= Colon equals sets value of parameter for a property or a method call.  

```
MsgBox "Hello World", , "Hello Title"  
MsgBox Prompt:="Hello World", Title:="Hello Title"   
```
Both lines in this example do the exact same thing. However, here we can see that using the colon and the title of the parameters being set, we can actually set them out of the specified order and we don't need to use the silly empty argument for style.  



## Functions[](#functions)  

Functions are pieces of reusable VBA code that **return values.** A function must be defined as the type that it will return. They also must be invoked with () at the end in contrast to subroutines.   

Optional arguments must come after the mandatory arguments.  

```
Function MyFunction (ByVal Parameter0 As Integer, ByVal Optional Parameter1 As String ) As Boolean  
. . .
myFunction = True
End Function  
```
This example shows a function with one mandatory and one optional parameter that are passed by value (copy) to the function. It also shows how a function returns a value, by assigning a value of the functions type to the function name.  


#### Invoking Functions  

```
Set object = MyFunction(argument0, argument1)  
variable = MyFunction(argument0, argument1)  
Module.MyFunction(argument0,argument1)  
```

Functions can be used as functions with assignments to objects (noted by set), assignments to variables (noted by just the equals sign) or you can throw the value it returns away by not assigning it to anything.  

### Function example  

This function creates a worksheet in the current workbook then takes a list of files (such as mary-loves-orange-cats-100 and suzie-hates-big-moths-200), then seperates those names by the dash, and loads each word into a seperate cell.  

```
Function file_list(ByVal sPath As String, ByVal primary As String) As Boolean  
    ' Creates worksheet in cheng code and  
    ' fills with file names in the selected directory  
    ' if no file names, it will return false.  
    On Error GoTo Catch
    ' Add worksheet code from:  
    ' http://codevba.com/excel/add_worksheet.htm#.XOQIfdpKiUk  
    Debug.Print "Creating file list... "  
    Dim wb As Workbook  
    Windows(primary).Activate  
    Set wb = ActiveWorkbook  
    Dim strName As String  
    strName = "temp"  
    Dim ws As Worksheet  
    Set ws = wb.Worksheets.Add(Type:=xlWorksheet)  
    ws.Name = strName  
    ' Cycle through files in folder from:  
    ' https://stackoverflow.com/questions/31414106/get-list-of-excel-files-in-a-folder-using-vba  
    Dim vaArray   As Variant  
    Dim i               As Integer  
    Dim oFile       As Object  
    Dim oFSO     As Object  
    Dim oFolder  As Object  
    Dim oFiles     As Object  

    Set oFSO = CreateObject("Scripting.FileSystemObject")  
    Set oFolder = oFSO.GetFolder(sPath)  
    Set oFiles = oFolder.Files  

    If oFiles.Count = 0 Then  
        file_list = False  
        Exit Function  
    End If  

    ReDim vaArray(1 To oFiles.Count)  
    Dim afterSplit() As String  
    Dim rangeString As String  
    Worksheets("filenames").Activate  
    i = 1  
    For Each oFile In oFiles  
        afterSplit = Split(oFile.Name, "-", 5)  
        rangeString = "A" & i & ":" & "E" & i  
        Range(rangeString).Value = afterSplit  
        i = i + 1  
    Next  
    file_list = True  
	End Function

	Catch:
		MsgBox "Oops! Here's what went wonky: " & Err.Description
		file_list = False
End Function

End Function
```



## Subroutines[](#subroutines)  

Subroutines are pieces of reusable VBA code that **do not** return values.  

```
Sub MyFunction (ByVal Parameter0 As Integer, ByVal Optional Parameter1 As String )
. . .
End Sub
```

You can see here that a sub can also take optional, mandatory, or no arguments at all, but it will never have a defined type or return a value.

#### Invoking subroutines  

```
MySubroutine argument0, argument1  
MsgBox "Hello world!", , "Hello"  
MsgBox Prompt:="Hello World", Title:="Hello Title"  
```
When you invoke a subroutine you don't use parentheses and you can never assign it to a variable, because your subroutine will never return a value. That's like, why you made it a subroutine.  

### Subroutine that invokes subroutines example

This is the code on an excel workbook's sheet1 to define what the button will do when it is pressed. Here, it will open a filedialog that accepts a directory. If nothing is selected, it will let the user know and exit. If anything else goes wrong, it will provide an error message to the user.

```
Private Sub go_button_Click()  
    Dim directory As FileDialog  
    Set directory = Application.FileDialog(msoFileDialogFolderPicker)  
    directory.AllowMultiSelect = False  
    On Error GoTo Catch  
    If directory.Show = True Then  
        ' Folder was selected (directory non empty), proceed.  
        Debug.Print directory.SelectedItems(1)  
        Dim path As String  
        Dim file As String  
        Application.ScreenUpdating = False  
        path = CStr(directory.SelectedItems(1))           ' Full path to selected directory as string  

		' Do some stuff here    

    Else  
        ' Nothing was selected.  
        Debug.Print "Nothing was selected"  
        MsgBox Prompt:="You didn't select anything. :(", Title:="Oops"  
		Exit Sub
    End If  
Exit Sub  
' Error catcher.  
Catch:  
	' If something went w
    MsgBox Prompt:="Something broke. " & Err.Description, Title:="Oops"  
End Sub  
```


## Debugging[](#debugging)  

Print to the Immediate window:  

```
debug.Print VariableICareAbout  
```

Disable alerts when doing stuff:

```
Application.DisplayAlerts = False     ' deactivate alerts  
Sheets("filenames").Delete						' delete my SHEET  
Application.DisplayAlerts = True      ' reactivate alerts  
```

Catch all errors thrown by a bit of code:

```
Sub cleaner(ByVal primary As String)
	On Error GoTo Catch
	' Deletes sacrificial worksheets, resets the gui stuff.
    Debug.Print "Tidying up... "
    Dim wb As Workbook
    Windows(primary).Activate
    Set wb = ActiveWorkbook
    Application.DisplayAlerts = False           ' deactivate alerts
    Sheets("temp").Delete
    Application.DisplayAlerts = True             ' reactivate alerts
Done:
    Exit Sub
Catch:
    MsgBox "Oops! Here's what went wonky: " & Err.Description
End Sub
```


## Excel Shortcuts[](#shortcuts)   

Ctrl + (any) arrow- key :  shift to the last filled cell on that row/column


## Credits[](#credits)  

Aka. What I used when I was learning this  

- [Tutorialspoint VBA](https://www.tutorialspoint.com/vba/vba_overview.htm)  
- [Microsoft MsgBox Function Documentation](https://docs.microsoft.com/en-us/office/vba/language/reference/user-interface-help/msgbox-function)  
- [Youtube: Recording Macros in Excel](https://www.youtube.com/watch?v=nvWpFdo7EO0)  
- [Difference Between Equal ‘=’ and ‘:=’ Colon Equal in VBA](https://www.excelcampus.com/library/vba-difference-equal-sign-colon-equal-sign/)  
- [Youtube: Excel VBA Introduction Part 24 - File Dialogs](https://youtu.be/6ZIFNAV1rOQ)  
- [Excel VBA Function Tutorial: Return, Call, Examples](https://www.guru99.com/vba-function.html)  
- [Excel VBA Range Object](https://www.guru99.com/vba-range-objects.html)  
- [Add new worksheet with name](http://codevba.com/excel/add_worksheet.htm#.XOQIfdpKiUk)  
