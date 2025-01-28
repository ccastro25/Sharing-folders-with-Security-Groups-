# Sharing-folders-with-Security-Groups-
This guide details the steps to create shared folders and assign permissions using Security Groups within an Active Directory environment. It covers partitioning drives, setting up security groups, creating and sharing folders, and mapping network drives for user access.


## Creating Drive for Shared Folders
1. Go to **Control Panel\System and Security\ Administrative tools**
   - Computer Management\Disk Management
   - Shortcut: `Win + X`, select Disk Management
2. Right-click the drive you want to partition
   - Select **Shrink Volume**
   - Enter the desired size for the shared drive
     - Enter size at **"Enter the amount of space to shrink in MB"**
     - Click **Shrink**
   - Unallocated Space will appear
     - Right-click the new Volume and select **New Simple Volume**
     - At the wizard window, click **Next** until you reach **Assign Drive Letter or Path**
     - Leave the default selection **"Assign the following drive letter"**
     - Select the desired letter for the drive and click **Next**
     - Rename volume name to **Share** and leave everything else as default at **Format Partition**, then click **Next**
     - Lastly, click **Finish**


https://github.com/user-attachments/assets/fe645e9a-30f4-4f61-be6c-f18b003c53f9


## Creating Security Groups
1. Within **Server Manager**, select **Tools**
2. Select **Active Directory Users and Computers**
3. Right-click your domain
   - Select **New > Organizational Unit**
     - Name it **Security Groups**
   - Right-click **Security Groups**
     - Select **New > Organizational Unit**
       - Name it **Shared Folder**
     - Select **Shared Folder**
       - Right-click under **"There are no items to show in this view"**
         - Select **New > Group**
           - Select **Domain Local** under **Group scope**
           - Name Group **sf-admin**
       - Create 3 more Security Groups with the following names: 
         - **sf-Tech**
         - **sf-Sales**
         - **sf-Marketing**
       - Right-click **sf-admin** and select **Properties**
         - Select **Members** tab
         - Click **Add**
           - Enter **administrator** under **"Enter the object names to select"**
           - Click **OK**
           - Hit **Apply** and **OK** at the properties window
       - Follow the same steps for **sf-Marketing**, **sf-Sales**, and **sf-Tech**
         - Right-click the group and select **Properties**
           - Select **Members** tab
           - Click **Add**
             - Enter the user name under **"Enter the object names to select"**
             - Click **OK**
             - Hit **Apply** and **OK** at the properties window

## Creating Folders
1. Open **File Explorer**
2. Enter the desired drive
3. Create a folder and name it **Share**
4. Create 3 folders inside **Share** and name them:
   - **IT**
   - **Sales**
   - **Marketing**

## Share Properties
1. Right-click the **Share** folder and select **Properties**
   - Select the **Security** tab
   - Click **Advanced** button
     - At the new window, click on **Change** to change Owner
       - Enter **sf-administrator** at **"Enter the object name to select"**
       - Click **OK**
     - Click **Disable Inheritance**
       - Select **"Convert inherited permissions into explicit permissions on this object"**
     - Remove all Permissions entries except **System**
     - Select **Add**
       - Select **"Select a principal"**
         - Enter **sf-administrator** at **"Enter the object name to select"**
         - Click **OK**
         - Select **Full control** at **Basic permissions**
         - Click **OK**
     - Select **Add**
       - Enter **domain users**
       - Click **OK**
       - Leave **Read & execute**, **List folder contents**, and **Read**
       - Click **OK**
   - Hit **Apply** followed by **OK**
   - Hit **OK** at the Windows Security pop-up
   - Hit **OK** at the shares properties window
   - Log off and back on

## Setting Permissions for Subfolders
1. Enter the **Share** folder
2. Right-click the **IT** folder and select **Properties**
   - Select the **Security** tab
   - Click the **Advanced** button
     - Click **Disable Inheritance**
       - Select **"Convert inherited permissions into explicit permissions on this object"**
     - Remove all Permissions entries except **System** and **sf-admin**
     - Select **Add**
       - Select **"Select a principal"**
         - Enter **sf-tech** at **"Enter the object name to select"**
         - Click **OK**
         - Select **Modify**, **Read & execute**, **List folder contents**, **Read**, and **Write**
         - Click **OK**
   - Hit **Apply** followed by **OK**
   - Hit **OK** at the Windows Security pop-up
   - Hit **OK** at the shares properties window
3. Repeat the same steps for the remaining 2 folders:
   - At the **Sharing** tab:
     - Share the **Marketing** folder with **sf-Marketing**
     - Share the **Sales** folder with **sf-Sales**
   - At the **Security** tab, grant permission:
     - The **Marketing** folder to **sf-Marketing**
     - The **Sales** folder to **sf-Sales**

## Mapping Drive
1. Within **Server Manager**, select **Tools**
   - Select **Group Policy Management**
   - Under your domain:
     - Right-click **Group Policy Objects**
       - Select **New**
       - Name the new Object
       - Click **OK**
     - Right-click the created Object
       - Select **Edit**
         - Under **User Configuration**:
           - Go to **Preferences > Windows Settings**
           - Select **Drive Maps**
             - Right-click under **"There are no items to show in this view"**
               - Select **New > Mapped Drive**
               - New Drive Properties Window should appear
                 - Under **General** tab, paste the location of Drive at **"Location"**
                   - To find the location:
                     - Go to the **Shares** folder
                     - Right-click and select **Properties**
                       - Under the **Shares** tab:
                         - In the **Network File and Folder Sharing**:
                           - The path will be under **"Network Path:"**
                 - Under **Drive Letter**, select **"Use"** and pick a drive letter
                 - Hit **Apply** then **OK**
   - Return to the **Group Policy Management** window
   - Drag the new GPO onto your domain
     - Click **OK** when prompted

## Testing Shared Folders
1. Log on to the Windows VM

## Deleting within Active Directory Users and Computers
1. Go to **View**
   - Select **Advanced Features**
     - Right-click the desired folder
       - Select **Properties**
       - Select the **Object** tab
         - Uncheck **Protected object from accidental deletion**
         - Click **Apply** and **OK**
     - Right-click the item you want to delete
       - Select **Delete**
       - Click **Yes** at the pop-up
       - If the selected item contains objects:
         - Select **Use Delete Subtree server control** at the bottom
         - Lastly, select **Yes**
