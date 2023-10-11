/******************************************************************************
Project 3

Author: Zeel Patel
Editor: Zybooks Lab
Version: August 23, 2023 
Date: Oct 9, 2023
Class: CS 141, Fall 2023

Purpose: I created a modified version of the game Rush Hour. I have coded a user input game where the goal is to get the red car beside the equal sign by moving the rest of the cars in any directions. 
The user can input which board they want to play in and that board is then displayed accordingly. Afterwards, the user can input which car can move x number of spaces towards a particular direction. My code then
proceeds to take specific steps to execute the inputed command. My code first checks which direction the car inputed is facing towards (is it an up/down car or a right/left car), using that information, the car is
tested for how many spaces out of the x spaces enetered can it move (to check for obstacles such as other cars blocking the way or out of bounds). After this information is collected, then the algorithem based on 
the cars direction (up/down facing or left/right facing) the code loops through either the rows or columns and swaps the car letters to their coresponding spots. The new board is then displayed and the process
is repeated for the user to play the game until they either quit or win.

*******************************************************************************/
#include <iostream>
#include <fstream>
#include <vector>
#include <cctype>

using namespace std;
//This function takes in the empty board and when the program starts, it populates the board for the first time with whatever was written in the file.
//Parameters includes a passed by reference carLetters string which stores the total letters that there are in the board so it can be compared to with
//user input later on. And the other parameter is the fileName that was inputed by the user after running the project. The function does not return anything
//but updates the baord 2d array which is then used later on in the code. 
void getAndStoreData(char board[][6],string& carLetters,string fileName){
    
    //Code provided by teacher to class, I modified it to loop through the characters of every line from getline() so that it can be stored in the board array.
    
    //variables to read in file
    string someData;//stores the lines read from file 
    ifstream inFS; 

    //Keeps track of the row index value of the board so that the board letters and dashes get populated in the right spots
    int boardRowIndex = 0;

    //opens the file with the user inputed name
    inFS.open(fileName);

    //Checks if such a file exists
    if(!inFS.is_open()){
        //No, invalid input
        cout << "Could not open file." << endl;
    }
    do{
        //valid input

        //gets the line by line from the file per itteration
        getline(inFS,someData);

        for(int i = 0; i < 6; ++i){//loops through the length of someData var which is going to be 6 because that is how big the board will be

            board[boardRowIndex][i] = someData.at(i);//Adds the values of each character into the cooresponding index on the board
        }

        boardRowIndex++;//increases board row index so that the correct values are being put in the correct spots.

        }while(!inFS.eof());

        //This is used to store the total letters of each car within the board to a string so that it can be used to check if the user inputed a valid car or not. 
        for(int i = 0; i < 6; ++i){
            for(int j = 0; j < 6; ++j){

                if(board[i][j] != '-'){
                    carLetters += board[i][j];//adds to the string if the character is a car letter 
                }
            }
    
        }
}

//This function checks if the car that is being moved that is inputed is the correct car on the baord or not. Returns true if car is on the board and false if the car is not on the board.
//Parameters include the string input of the user and the characters of the cars that are on the board which were stored in a string when board was initally populated but not modified. 
bool checkLetters(string moveInput,string carLetters){

    int totalLetterCount = 0;//Counter for each car that has been checked on the board
    int size = carLetters.size();//Size of the amount of individual car letters that are on the board

    //loops to check if the user input for which car should move is un equal to any of the cars on the board. Predicted to output the size of the carLetters variable if the car is not on the board
    for(int i = 0; i < size; i++){
        if(tolower(moveInput.at(0)) != tolower(carLetters.at(i))){
            totalLetterCount++;
        }

    }

    //checks to see if the counter that was incremented from the loop is equal to the size of the string with the total letters of each car populated in it.
    if(totalLetterCount == size){
        //Yes, then car is not on the baord and false is returned 
        cout << "That car is not on the board." << endl;
        return false;
    }else{
        //No, then car is on the baord and true is returned 
        return true;
    }
}

//This function is used to generate a copy of the board and later on in the code is displayed to the user each time that the user makes a move. Output is the board copy being displayed and has no return value.
//Parameters have the board 2d array which is unchanged and a copy board array which is changed. Purpose of function is to ensure that changing the characters on the board does not change the file.
void generateCopyBoard(char board[][6], char newCopy[][6]){

    //loops through the row and column of the 6 by 6 board
    for(int i=0;i<6;i++){
        for(int j=0;j<6;j++){

            newCopy[i][j] = board[i][j]; //copies the board

        }
    }

}

//This function prints the copy board that was generated from the origial board and displays it in the correct format. The output should be a printed board in the correct format of the project instructions. 
//Parameters only have the board 2d array that is to be printed out but not modified.
void printCopyBoard(char board[][6]){

    cout << "--------" << endl;//displays top of the board

        //loops through the rows and columns of the array 
        for(int i = 0; i < 6; ++i){//rows

            cout << "|";//left side straight wall that starts each row 

            for(int j = 0; j < 6; ++j){//column

                //checks to make sure that each of the dashes are replaced by a period when the board is printed
                if(board[i][j] == '-'){
                    cout << '.';
                }else{
                   //If the character is not a dash then it is a car and therefore the car is printed
                    cout << board[i][j];
                }
            }

            //Adds the finish line/equal sign where the red car needs to end up going to 
            if(i == 2){
                cout << "=";
                cout << endl;
               
            }else{
                //adds a wall if a equal sign is not added
                cout << "|";
                cout << endl;
              
            }
        }

        cout << "--------" << endl;//bottom of the board

}

//This function checks the direction of the car. Checks one of two possibilities, if it is a left/right car or a up/down car. This information is useful when moving the car hense why is it computed seperately. 
//Output should be a vector with all the information about the car that is being moved by the user, which row and column it is in, the length and which direction the car is in (1 for up/down car and 2 for right/left car). Parameters include the car that was inputed by the user to be moved and the game board.
vector<int> checkDirection(char car, char board[][6]){

    //Vector that is going to be returned
    vector<int>info = {0,0,0,0};
  
    //Used to determine which way the length of the car should be generated as checking direction for up/down car is different then right/left car
    bool checkDown = true;
    bool checkRight = true;

    //Used to keep track of the row and column of the car 
    int startingCol = -1;
    int startingRow = -1;

    //itterate through the board to find where the cars row and column is 
    for(int i=0;i<6;i++){//row

        for(int j=0;j<6;j++){//col

            //This is used to check if the correct car is found and if it is the first car letter in the series
            if( (board[i][j] == toupper(car)) && (startingCol == -1)){

                //If the car is found then the row and column is stored into the array and loop should stop
                startingCol = j;
                startingRow = i;
                
                break;
               
            }
        
        }
        
        //checks if the car is not found then to continue finding it otherwise to stop looking for it once either the column or row is found 
        if(startingCol != -1){
            break;
        }

    }

    //lowest the length of a car can be. Variable is updated based on the length of the car
    int carLength = 1;

    //If the car is a up/down car then the length of the car is measured a certain way 
    while(checkDown){

        //the row of the board is changed to check for the car length and for every car found in the increased row that is equal to the car that the user is going to move then the length variable is incremented.
        if(board[startingRow+carLength][startingCol] == toupper(car)){
            
            //continues looking for the car if the car is there 
            checkDown = true;
            //increases the length of the car var for every car found 
            carLength++;

        }else{
            //once the car is no longer found then the loop stops.
            checkDown = false;
    
        }
        

    }

    //If a car was found then the function stops and the vector is returned with the correct values placed in each index
    if(carLength != 1){

        info.at(0) = startingRow;//index 0 is the row that the car starts at 
        info.at(1) = startingCol;//index 0 is the column that the car starts at
        info.at(2) = 1; //1 to show that the car is a up/down at
        info.at(3) = carLength;//index 3 is the length of the car
        return info; //vector is returned
    }

    //Loops and checks if the car is a right/left direction car.
    while(checkRight){

        //the column of the board is changed to check for the car length and for every car found in the increased row that is equal to the car that the user is going to move then the length variable is incremented.
        if(board[startingRow][startingCol+carLength] == toupper(car)){

            //continues looking for the car if the car is there 
            checkRight = true;
            //increases the length of the car var for every car found 
            carLength++;

        }else{

            //once the car is no longer found then the loop stops.
            checkRight = false;
         
        }
        

    }

    //If a car was found then the function stops and the vector is returned with the correct values placed in each index
    if(carLength != 1){
      
        info.at(0) = startingRow;//index 0 is the row that the car starts at
        info.at(1) = startingCol;//index 0 is the column that the car starts at
        info.at(2) = 2; //2 to show that the car is a up/down at
        info.at(3) = carLength; //index 3 is the length of the car
        return info;//vector is returned 
    }

    return info;
}

//This function moves the car down or up based on which type of car it is (left/right or up/down). Output is an updated board with the specified car that has been moved, the updated board is then printed in the main 
//function. The parameters include of the amount of steps that the user will want to move the car, the board copy, the car information vector, and the direction of the car (up/down car or left/right car)
void moveDownOrUp(char steps, char board[][6],vector<int> information,string direction){

    int stepNew = steps -48;//converts the steps char into an integer

    int length = information.at(3);//length of car
    
    int row = information.at(0);//car row 
    int col = information.at(1);//car column
    int countDown = 0;//how many actual spaces the car can move down/up
    int count = 0;//the counter for the row/column the car will move on the board

    vector<int> moveSpaces;//Vector holds the total moves that each individual car letter can make and the lowest value is choosen
    int finalMoveSpaces = -1;//the final total amount of moves 

    if(information.at(2) == 2){//right facing cars 

        for(int i=col;i<length + col;i++){//iterates through the columns 

            countDown = 0;

            for(int j=1;j<=stepNew;j++){//loop that checks how many actual spaces are availible to move (incase of cars or out of board issues)

                if(row+j <= 5 && board[row+j][i] == '-' && direction == "down"){//if there is an empty space 

                    countDown++; 

                }else if(row+j <= 5 && board[row+j][i] != '-' && direction == "down"){//if there is a wall
                
                    break;
                }
                
                if (row-j >= 0 && board[row-j][i] == '-' && (direction == "up")){//if there is an empty space

                    countDown++;

                }else if(row-j >= 0 && board[row-j][i] != '-' && (direction == "up")){//if there is a car
                   
                    break;
                }
            }

            moveSpaces.push_back(countDown);//adds the amount of spaces per letter of the car that the car can move

            finalMoveSpaces = *min_element(moveSpaces.begin(), moveSpaces.end());//Takes the minimum amount of spaces that can be moved
        }

        //moves the car through swapping based on the finalMovesSpaces and the row and column of the car 
        while(length > 0){

            int tempIndRow = row;
            int tempIndCol = col + count;
            int aditor = 0;

            char holdVal = board[tempIndRow][tempIndCol]; //holds the value of the first index of the car

            if(direction == "down"){//to move it down
                aditor = tempIndRow + finalMoveSpaces;
            }else if(direction == "up"){//to move it up
                aditor = tempIndRow - finalMoveSpaces;
            }

            //swapping the values
            if(board[aditor][tempIndCol] == '-'){

                board[aditor][tempIndCol] = holdVal;
                board[tempIndRow][tempIndCol] = '-';

            }

            //does this process for the entire car
            count++;
            length--;

        }
    }else if(information.at(2) == 1){//downward facing car 

        for(int j=1;j<=stepNew;j++){//loop that checks how many actual spaces are availible to move (incase of cars or out of board issues)

            if(row+j+length-1 <= 5 && board[row+j+length-1][col] == '-' && direction == "down"){//if there is an empty space

                countDown++;

            }else if(row+j+length-1 <= 5 && board[row+j+length-1][col] != '-' && direction == "down"){//if there is a wall
                break;
            } 
                
            if (row-j >= 0 && board[row-j][col] == '-' && direction == "up"){//if there is an empty space
            
                countDown++;

            }else if(row-j >= 0 && board[row-j][col] != '-' && direction == "up"){//if there is a wall
                break;

            }

        }
            
        //moves the car through swapping based on the finalMovesSpaces and the row and column of the car 
        while(length > 0){

            int tempIndRow = row + count;
            int tempIndCol = col;
            int aditor = 0;
            
            char holdVal = board[tempIndRow][tempIndCol]; //holds the value of the first index of the car

            if(direction == "down"){ //move car down
                aditor = tempIndRow + length - 1 + countDown; //holds the value of where the car will go
            }else if(direction == "up"){ //move car up
                aditor = tempIndRow - countDown; //holds the value of where the car will go
            }

            //swapping the values
            if(board[aditor][tempIndCol] == '-' ){

                board[aditor][tempIndCol] = holdVal;
                board[tempIndRow][tempIndCol] = '-';

            }

            count++;
            //does this process for the entire car based on the car direction certain variables have to be decremented
            if(direction == "down"){
                stepNew--;
                countDown--;
                length--;
            }else if(direction == "up"){
                length--;
            }
                
                
        }  
        
    }

}

//This function moves the car left or right based on which type of car it is (left/right or up/down). Output is an updated board with the specified car that has been moved, the updated board is then printed in the main
//function.The parameters include of the amount of steps that the user will want to move the car, the board copy, the car information vector, and the direction of the car (up/down car or left/right car)
void moveLeftOrRight(char steps, char board[][6],vector<int> information,string direction){

    int length = information.at(3);//length of car
    int stepNew = steps -48;//converts the steps char into an integer
    int row = information.at(0);//car row 
    int col = information.at(1);//car column
    int countLeft = 0;//how many actual spaces the car can move down/up
    int count = 0;//the counter for the row/column the car will move on the board

    vector<int> moveSpaces;//Vector holds the total moves that each individual car letter can make and the lowest value is choosen
    int finalMoveSpaces = -1;//the final total amount of moves 

    if(information.at(2) == 1){//down facing cars 

        for(int i=row;i<length+row;i++){ //loops through rows since the car is a left/right facing car

            countLeft = 0;

            for(int j=1;j<=stepNew;j++){ //loop that checks how many actual spaces are availible to move (incase of cars or out of board issues)

                if(col-j >= 0 && board[i][col-j] == '-' && direction == "left"){//if there is an empty space

                    countLeft++; 
                   
                }else if(col-j >= 0 && board[i][col-j] != '-' && direction == "left"){//if there is a wall
             
                    break;
                }
                
                if ((col+j <= 5) && (board[i][col+j] == '-') && (direction == "right")){//if there is an empty space

                    countLeft++;

                }else if((col+j <= 5) && (board[i][col+j] != '-') && (direction == "right")){//if there is a wall
                    
                    break;
                }
            }

            moveSpaces.push_back(countLeft);//adds the amount of spaces per letter of the car that the car can move

        }

        finalMoveSpaces = *min_element(moveSpaces.begin(), moveSpaces.end());//Takes the minimum amount of spaces that can be moved

            //moves the car through swapping based on the finalMovesSpaces and the row and column of the car 
        while(length > 0){

            int tempIndRow = row + count;//row is incremented to move the up/down facing car 
            int tempIndCol = col;
            int aditor = 0;
            
            char holdVal = board[tempIndRow][tempIndCol]; //holds the value of the first index of the car

            if(direction == "left"){//move car left
                aditor = tempIndCol - finalMoveSpaces;//holds the value of where the car will go
            }else if(direction == "right"){//move car right
                aditor = tempIndCol + finalMoveSpaces;//holds the value of where the car will go
            }

            //swapping the values
            if(board[tempIndRow][aditor] == '-'){

                board[tempIndRow][aditor] = holdVal;
                board[tempIndRow][tempIndCol] = '-';

            }
                
                //repeats for the length of the cars 
            count++;
            length--;

        }
        
    }else if(information.at(2) == 2){//right facing cars 

        for(int j=1;j<=stepNew;j++){//loop that checks how many actual spaces are availible to move (incase of cars or out of board issues)

            if(col-j >= 0 && board[row][col-j] == '-' && direction == "left"){//if there is an empty space
                countLeft++;
            }else if(col-j >= 0 && board[row][col-j] != '-' && direction == "left"){//if there is an wall
                break;
            } 
                
            if (col+length+j-1 <= 5 && board[row][col+length+j-1] == '-' && direction == "right"){//if there is an empty space
                   
                countLeft++;
            }else if(col+length+j-1 <= 5 && board[row][col+length+j-1] != '-' && direction == "right"){//if there is an wall
                break;
            }

        }

        //moves the car through swapping based on the finalMovesSpaces and the row and column of the car 
        while(length > 0){

            int tempIndRow = row;
            int tempIndCol = col + count;

            char holdVal = board[tempIndRow][tempIndCol]; //holds the value of the first index of the car

            int aditor = 0;

            if(direction == "left"){//move car left

                aditor = tempIndCol-countLeft;//holds the value of where the car will go

            }else if (direction == "right"){//move car right

                aditor = tempIndCol+length-1+countLeft;//holds the value of where the car will go
            }
            
            //swapping the values
            if(board[tempIndRow][aditor] == '-'){

                board[tempIndRow][aditor] = holdVal;
                board[tempIndRow][tempIndCol] = '-';

            }

            //repeats for the length of the car depending on car direction
            count++;

            if(direction == "right"){
                stepNew--;
                countLeft--;
                length--;   
            }else if (direction == "left"){
                    
                length--;
            } 

        }

    }
}

//This function checks if the user has won or not. The output of the function should be a display that the user won and returns true or returns false if the user has not won.
//Parameters include the board 2d array
bool checkWin(char board[][6]){

    //Checks to see if there is a red car stationed beside the equal sign
    if(board[2][5] == 'R'){
        //Yes, then print the board one last time, display congrats and return true so that the game stops
        printCopyBoard(board);
        cout << "You win! Congratulations!" << endl;
        return true;
    }else{
        //No, then return false so that the game continues 
        return false;
    }


}

//This is the main function that runs the game and it has no returns but it should output several things based on the conditions of the game. If the user has not won then the function
//should continue to print out the board and asking for user input. If the user has won then the function will stop and then game will end. 
int main()
{

    char board[6][6];//Original Board
    char newCopy[6][6];//Copy Board

    vector<int> inpCarInformation;//Vector that stores the inputed car information 
    inpCarInformation.reserve(2);//reserves 2 spots within the array so that it can be updated when the the direction of the car is checked

    //The variable that stores the user input for car direction
    string moveInp;
    bool win = false;//Condition which checks the repetivity of the game 
    string boardFileName; //Name for the inputed board
    string carLetters = ""; //total letters for the bio

    //get file name from input 
    cout << "Enter filename: ";
    cin >> boardFileName;
    cout << endl;

    //generates intital board and copies it 
    getAndStoreData(board,carLetters,boardFileName);
    generateCopyBoard(board,newCopy);

    //continues to loop until the user has won or quit the game 
    while(!win){

        printCopyBoard(newCopy); //prints the board 

        //user enters move
        cout << "Enter next move (or Q to quit): ";
        cin >> moveInp;

        //quit condition
        if(moveInp == "Q"){
            cout << "Program quiting" << endl;
            win = true;
        }else{

            //checks if the letters are on the board
            if(checkLetters(moveInp,carLetters)){

                //checks for down direction
                if(tolower(moveInp.at(2)) == 'd'){

                    inpCarInformation = checkDirection(moveInp.at(0),newCopy);//checks the direction of the car and the returned vector is set to the local vector

                    moveDownOrUp(moveInp.at(1),newCopy,inpCarInformation,"down");//moves the car down 
                    
                    win = checkWin(newCopy);//checks for the car to win 

                }else if(tolower(moveInp.at(2)) == 'u'){//checks for up direction

                    inpCarInformation = checkDirection(moveInp.at(0),newCopy);//checks the direction of the car and the returned vector is set to the local vector

                    moveDownOrUp(moveInp.at(1),newCopy,inpCarInformation,"up");//moves the car up 

                    win = checkWin(newCopy);//checks for the car to win 

                }else if (tolower(moveInp.at(2)) == 'l'){//checks for left direction

                    inpCarInformation = checkDirection(moveInp.at(0),newCopy);//checks the direction of the car and the returned vector is set to the local vector

                    moveLeftOrRight(moveInp.at(1),newCopy,inpCarInformation,"left");//moves the car left 

                    win = checkWin(newCopy);//checks for the car to win 

                }else if(tolower(moveInp.at(2)) == 'r'){//checks for right direction

                    inpCarInformation = checkDirection(moveInp.at(0),newCopy);//checks the direction of the car and the returned vector is set to the local vector

                    moveLeftOrRight(moveInp.at(1),newCopy,inpCarInformation,"right");//moves the car right 

                    win = checkWin(newCopy);//checks for the car to win 
                }                

            }

        }

    }


    return 0;
}
