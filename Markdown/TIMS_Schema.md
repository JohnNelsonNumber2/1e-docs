# SCHEMA
Every instruction you write for Endpoint Troubleshooting (a Question or Action) and every automation you write for Endpoint Automation (a Check, Fix or PreCondition) must have a schema created for it.  

The schema tells the platform what the shape of the data is that's being returned from the 1E Client.
It defines the names of the columns, as well as the data types.  
  
Each column can have any of the following data types  
![image](https://github.com/user-attachments/assets/9067c9ac-873a-4d88-918f-683041dbfb90)

## Defining a schema for a Question or Action
1. Click on `Schema` in the toolbar ribbon at the top of TIMS, you'll see the `Edit Schema` dialog.  
![image](https://github.com/user-attachments/assets/b8cb96b5-03c4-486a-81a1-7038c5aa27a8)  
2. If you've got data in the grid at the bottom of TIMS already, you may see a popup that says <!-- There is no schema defined for this instruction. Do you want to create one based on the data that your instruction has returned? -->
![image](https://github.com/user-attachments/assets/a9e0f853-47cb-428c-ace4-bb574c809932)  
If you click `Yes` it will automatically populate the schema with something that matches the data in the grid  
![image](https://github.com/user-attachments/assets/2c1b455b-fa08-4387-aeb7-9f2966400a79)
3. The column names in the schema have to match the column names emitted from the instruction  
> [!NOTE]
> Make sure the data types are correct, especially the string widths. Think about the kinds of data that could possibly be returned from each column and if the string needs to be widened to accommodate data from different kinds of devices in the enterprise, make the change in the `Length` field for that column.
>  
> The data defined by this schema will show up in `Endpoint Troubleshooting` as the content returned from running a question or action
  
## Defining a schema for Check, Fix or PreCondition
> [!NOTE]
> Every Endpoint Automation check, fix or precondition needs to have the exact same schema.  
> &nbsp;&nbsp;&nbsp;&nbsp;A `Passed` column that's an `int32` or `int64`  
> &nbsp;&nbsp;&nbsp;&nbsp;A `Data` column that's a `string` that's `1024` characters or less


1. Click on `Schema` in the toolbar ribbon at the top of TIMS, you'll see the `Edit Schema` dialog.  
![image](https://github.com/user-attachments/assets/b8cb96b5-03c4-486a-81a1-7038c5aa27a8)
2. If you've got data in the grid at the bottom of TIMS already, you may see a popup that says <!-- There is no schema defined for this instruction. Do you want to create one based on the data that your instruction has returned? -->
![image](https://github.com/user-attachments/assets/a9e0f853-47cb-428c-ace4-bb574c809932)  
If you click `Yes` it will automatically populate the schema with something that matches the data in the grid  
![image](https://github.com/user-attachments/assets/e92e9353-1825-4345-8032-52c1dbda4333)
3. Make sure there is a `Data` column that's `1024` characters or less
4. Make sure there is a `Passed` column that's `int32` or `int64`
  
> [!NOTE]
> 1.  Endpoint Automation data that's returned from the 1E client by a Check, Fix or PreCondition will be automatically truncated by the platform if it's greater than 1024 characters.
>   
> 2.  Make sure your data fits within 1024 or less to avoid truncation.
>
> 3.  The data for this schema will show up in `Endpoint Automation`.  There will be a `View History` link when you drill down into the details of a rule for a particular device.  This will show the status and the data returned from the device.  
>
> 4.  Returning data from an `Endpoint Automation` check or fix is a quick way to return data from a device that will survive even if the device is offline
