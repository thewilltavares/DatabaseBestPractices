## **Naming and Renaming**
<ul>
<li>The name of an object definition script should always match the name of the object.</li>  
<li>If an object is renamed, then the script for the “old” name should contain only an “IF EXISTS THEN DROP” command.</li>  
<li>The script for the “new” name should contain an “IF EXISTS THEN DROP” only for the new name.</li>  
<li>Do not drop the old object name in the new object name script.</li>
</ul>

## **Modifying an Existing Object**
<ul>
<li>When modifying a previously existing object, always branch the object from a <b>Production</b> subfolder (current <b>Production</b> path is <b>$/Database/Source/Production</b>) to the folder in the environment where initial development will occur.</li>  
<li>Add a comment to the modification log near the beginning of the object definition.</li>  
<li>If a modification log does not exist, then create one.</li>  
<li>Reference a VSTS User Story or Task number in the modification log when appropriate.</li>
</ul>

## **Adding a New Object**
<ul>
<li>When creating a new object, add a script for the object to the folder in the environment where initial development will occur.</li>  
<li>Include a “DROP IF EXISTS” command at the beginning of the script.</li>  
<li>Include a modification log comment area near the beginning of the object definition.</li>  
<li>Reference a VSTS User Story or Task number in the modification log when appropriate.</li>  
<li>Include any necessary GRANTs after the object definition.</li>  
<li>End the script with a “GO”.</li>
</ul>

## **Scripting a One-Time Data or Table Schema Modification**
<ul>
<li>Add the script to the folder in the environment where initial development will occur.</li>  
<li>When scripting a data modification, table change, or other one-time script, write the script so that it can be executed multiple times without error and without failing a unit or system test.  Such a script is called "idempotent", meaning that the result is unchanged no matter how many times a script is executed.</li>  
<li>Include comments that describe the purpose of the modification and explain the steps involved.</li>  
<li>Reference a VSTS User Story or Task number in the comments when appropriate.</li>  
<li>End the script with a “GO”.</li>
</ul>


