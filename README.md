# WeisiyuanA1_1404

Brief Program Details:
•The program starts by loading a CSV (Comma Separated Values) file of items that are in stock for hire. 
•Listing all items. Use indication (*) if they are currently out on hire. 
•Hiring an item should present a list of only the items currently available for hire along with their item number (the list index) for selection.
•Returning an item should present a list of only the items currently on hire (and so able to be returned) along with their item number for selection.
•When hiring, if no items are available then a message should be displayed and the user taken back to the menu.
•When returning, if no items are on hire then a message should be displayed and the user taken back to the menu.
•Adding an item should prompt for the item’s name, description and price, 
 which are all error checked, then the item should be added to the list of items in stock
•When the user quits, the program should save the items to the CSV file, overwriting any previous information.

Link of Github:

"""
#import csv library for Quit() to write in csv file 
import csv

#load file and return the file content as list of tuples to main() for furture action
def Loading_items_from_file():
    #if the file is not found, FileNotFoundError exception triggered
    try:
        file_read = open("items.csv", "r")
        content = file_read.readlines()
        #split strings and make as tuple
        file_content = [tuple(x.split(',')) for x in content]
        #close the file
        file_read.close()
        return file_content
    except FileNotFoundError:
        print("Sorry, cannot find the file")

#initial function that loads when the program runs   
def main():
   read_file_content = Loading_items_from_file()
   #For efficiency, only load the items file once and store the content in file_content global variable. 
   global file_content 
   file_content = read_file_content
   
   #List items based on conditions All/In/Out for different options
   def List_all_items(status):
        index = 0
        for content in file_content: 
            if status == 'All':   
                print(index, '-', content[0],
                '(', content[1], ')', 
                ' =$', '{:.2f}'.format(float(content[2])), 
                ("*" if content[3] == 'out\n' else " "))  
            elif status == 'In':
                if content[3] == 'in\n':    
                    print(index, '-', content[0],
                    '(', content[1], ')', 
                    ' =$', '{:.2f}'.format(float(content[2])), 
                    ("*" if content[3] == 'out\n' else " "))  
                    
            elif status == 'Out':
                if content[3] == 'out\n':      
                    print(index, '-', content[0],
                    '(', content[1], ')', 
                    ' =$', '{:.2f}'.format(float(content[2])), 
                    ("*" if content[3] == 'out\n' else " "))  
            index+=1
    
   #Allow user to view the avaliable items and select the item to hire
   def Hire_item():
        global file_content
        #since the tuple is immutable, convert the tuple list to list...
        file_content_list = [list(x) for x in file_content]
        List_all_items('In')
        #get number of avaliable items
        num_of_avaliable = len([x for x in file_content_list if 'in\n' in x])
        #if the number of avaliable item is 0, show error message
        if num_of_avaliable == 0:
                print("No items are currently avaliable")
        else:
            #if the number of hire item entered is not a integer, valueError exception triggered 
            try:
                hire_input = input("Enter the number of an item to hire: ") 
                #define a boolean check for check the status of hire item
                #if hire successfully, stop loop
                #if not hire successfully, show error message
                isValid = True            
                for i in range(len(file_content_list)): 
                   if isValid:            
                        if i == int(hire_input):
                            if file_content_list[i][3] == "in\n":
                               #change the status of the item to 'out' which is no longer able to be hired
                               file_content_list[i][3] = "out\n"
                               print(file_content_list[i][0],
                               "hired for $" + '{:.2f}'.format(float(file_content_list[i][2])))                  
                            else: 
                               print("That item is not available for hire")
                               
                   else:
                        print("That item is not available for hire")
                        isValid=False
            except ValueError:
                 print("Please enter a valid number")
        #convert the list back to tuple for furture action
        file_content = [tuple(x) for x in file_content_list]
    
   #Allow user to view the hired items and select the item to return     
   def Return_item():
        global file_content
		#since the tuple is immutable, convert the tuple list to list...
        file_content_list = [list(x) for x in file_content]
        List_all_items('Out')
        #get number of hired items
        num_of_hired = len([x for x in file_content_list if 'out\n' in x])
         #if the number of hired item is 0, show error message
        if num_of_hired == 0:
                print("No items are currently on hire")
        else:
            #if the number of return item entered is not a integer, valueError exception triggered 
            try:
                hire_input = input("Enter the number of an item to return: ")
                #define a boolean check for check the status of return item
                #if return successfully, stop loop
                #if not return successfully, show error message
                isValid = True 
                for i in range(len(file_content_list)): 
                    if isValid: 
                        if i == int(hire_input):
                            if file_content_list[i][3] == "out\n":
                                #change the status of the item to 'in' which is no longer able to be returned
                               file_content_list[i][3] = "in\n"
                               print(file_content_list[i][0], "returned")       
                            else: 
                               print("That item is not available to return")
                           
                    else:
                        print("That item is not available for hire")
                        isValid=False
            except ValueError:
                print("Please enter a valid number")
	  #convert the list back to tuple for furture action
        file_content = [tuple(x) for x in file_content_list]
                
   #Add new item to the list of items    
   def Add_item():
       #prompt for item details
       item_name = input("Item name: ")
       item_desc = input("Description: ")
       
        #if the price entered is not a integer, valueError exception triggered       
       try:     
           item_price = input("Price per day: $")
           #add the new_item tuple to the file_content   
           if item_price.isdigit(): 
               new_item = (item_name, item_desc , item_price , 'in\n')
               file_content.append(new_item)
               #successful message       
               print(item_name, 
               '(',item_desc,'), ', 
               '$','{:.2f}'.format(float(item_price)), 
               'now available for hire')  
           else:
               print("Please enter a valid price")
       except ValueError:
           print("Please enter a valid price")
           
       
    
    #quit the programs and store the updated values inside items.csv file
   def Quit():
        global file_content
		#convert tuple to list
        file_content_list = [list(x) for x in file_content]
        #remove all '\n'
        new_file_content = [[x.replace('\n','') for x in l] for l in file_content_list]
        #open file to write new content in
        with open('items.csv','w') as out:
			#write in file, assign delimiter ',' and line terminator '\n'
            csv_out=csv.writer(out, delimiter=',', lineterminator='\n')
            #write in file row by row
            for row in new_file_content:
                csv_out.writerow(row)
                
        #print the successful message
        print(len(file_content_list), "items saved to items.csv")
        print("Have a nice day :)")
        
   #boolean check initialized as false
   stop = False
    
   #if the boolean check is not true means not stop
   #keep prompting for selections if not stop 
   while not stop:
        #Menu bar
        menu = "Menu:\n(L)ist all items\n(H)ire an item\n(R)eturn an item\n(A)dd new item to stock\n(Q)uit"
        print(menu)
        
        #if the input is not a valid option, exception triggered
        try:
            #input a choice
            my_input = input(">>> ")
            
            #different options based on input
            if my_input in ['l', 'L']:
               List_all_items('All')
            elif my_input in ['h', 'H']:
               Hire_item()
            elif my_input in ['r', 'R']:
               Return_item() 
            elif my_input in ['a', 'A']:
               Add_item()
            elif my_input in ['q', 'Q']:
               Quit()
               stop = True
               
            #if the input is none of the charaters above
            else:
                print("Invalid menu choice")
        except:
            print("Invalid menu choice.")

#inilialization of main() function
if __name__ == "__main__":
    main()
