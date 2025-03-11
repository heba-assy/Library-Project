#include <iostream>

using namespace std;

//function for is the book is available or not.
bool isBookAvailable(int* ids, int bookCount, int& id, int &index){
  for(int i=0; i<bookCount; i++){
    if(ids[i]==id){
        index=i;
        return true;
    }
  }
  return false;
};

//function for adding a book
void addBook(int* ids, string* titles, string* authors, bool* availability, int &bookCount){
  int id, index=-1;
  string author;
  string title;
  cout<<"Enter the book ID: ";
  cin>>id;
  if(isBookAvailable(ids, bookCount, id, index)==true){
    cout << "The book is already in the library!\n";
    return;
  }
  cout<<"Enter the book author: ";
  cin.ignore();
  getline(cin,author);
  cout<<"Enter the book title: ";
  getline(cin,title);
  ids[bookCount]=id;
  titles[bookCount]=title;
  authors[bookCount]=author;
  availability[bookCount] = true;
  bookCount++;
  cout << "Book added successfully!"<<endl;
};
//Function for Searching
void searchBook(int* ids, int bookCount){
   int index=-1;
    int id;
    cout<<"Enter the ID of the Book: ";
    cin>>id;

    if(isBookAvailable(ids,bookCount,id,index)==false){
        cout<<"Error: Book ID Not Found!\n";
        return;
    }
    cout<<"Its position: "<<index<<endl;
};
//Function for removing a book
void removeBook(int* ids, string* titles, string* authors, bool* availability, int &bookCount){
     int idToRemove;
     cout<<"Enter the id of the book to remove: ";
     cin>>idToRemove;
     int indexToRemove = -1;
     for(int i=0; i<bookCount; i++){
        if(ids[i]==idToRemove){
            indexToRemove=i;
            break;
        }
     }
     if(indexToRemove==-1){
        cout<<"ERROR: Book with Id "<<idToRemove<<" Not Found!"<<endl;
     }
     for(int i=indexToRemove; i<bookCount-1; i++){
        ids[i]=ids[i+1];
        titles[i]=titles[i+1];
        authors[i]=authors[i+1];
        availability[i]=availability[i+1];
     }
     bookCount--;
     cout<<"Book with ID "<<idToRemove<<" has been removed successfully!"<<endl;

};

//Updating the Book
void updateBook(int* ids, string* titles, string* authors,bool* availability, int bookCount){
    int id, index = -1;
    cout<<"Enter the book ID: ";
    cin>>id;
     if(isBookAvailable(ids, bookCount, id, index)==false){
        cout<<"ERROR: Book with This Id Not Found!"<<endl;
        return;
     }
     int choice;
     do{
        cout<<"----which updates do you want----\n";
        cout<<"1. Author\n";
        cout<<"2. Title\n";
        cout<<"3. Both of them\n";
        cout<<"4. Exit\n";
        cout<<"Enter your choice: \n";
        cin>>choice;

        string newAuthor,newTitle;
        switch(choice){
           case 1:
               cout<<"Enter the new author: ";
               cin.ignore();
               getline(cin,newAuthor);
               authors[index]=newAuthor;
               break;
           case 2:
               cout<<"Enter the new title: ";
               getline(cin,newTitle);
               titles[index]=newTitle;
               break;
           case 3:
               cout<<"Enter the new author: ";
               cin.ignore();
               getline(cin,newAuthor);
               authors[index]=newAuthor;
               cout<<"Enter the new title: ";
               getline(cin,newTitle);
               titles[index]=newTitle;
               break;
           case 4:
               cout << "Exiting update process.\n";
               break;
           default:
              cout << "Invalid choice. Please try again.\n";
        }
        cout << "Book updated successfully!\n";
        return;
     }while(choice != 4) ;
}
//Borrow the Book
void borrowBook(int* ids, string* titles, string* authors, bool* availability, int bookCount){
    int id, index = -1;
    cout<<"Enter the book ID: ";
    cin>>id;
     if(isBookAvailable(ids, bookCount, id, index)==false){
        cout<<"ERROR: Book with This Id Not Found!"<<endl;
        return;
     }
     if(availability[index]==false){
        cout << "Sorry, this book is currently borrowed by someone else.\n";
        return;
     }
     availability[index] = false;
    cout << "You have successfully borrowed the book: \"" << titles[index] << "\" by " << authors[index] << ".\n";
};

//Returning the Book
void returnBook(int* ids,bool* availability, int bookCount){
    int id,index=-1;
    cout<<"Enter the book ID You have borrowed it: ";
    cin>>id;
    if(isBookAvailable(ids, bookCount, id, index)==false){
        cout<<"ERROR: Book with This Id Not Found!"<<endl;
        return;
     }
     availability[index]=true;
     cout << "The book with ID " << id << " has been successfully returned!\n";

};
//Display Books
void displayBooks(int* ids, string* titles, string* authors, bool* availability, int bookCount){
    if(bookCount==0){
        cout<< "No books available in the library.\n";
        return;
    }
    cout << "Available Books in the Library:\n";
    cout << "--------------------------------\n";
    cout << "ID\tTitle\t\t\tAuthor\t\tAvailability\n";

    for(int i=0; i<bookCount; i++){
        cout<<ids[i]<<"\t"<<titles[i]<<"\t\t"<<authors[i]<<"\t\t";
        if(availability[i]){
            cout<<"Available\n";
        }
        else{
            cout<<"Borrowed\n";
        }
    }
    cout << "--------------------------------\n";
};

//Menu function
void menu(int* ids, string* titles, string* authors, bool* availability, int bookCount){
    int choice;
    do{
        // Display menu options
        cout << "\n--- Library Menu ---\n";
        cout << "1. Add a Book\n";
        cout << "2. Search a Book\n";
        cout << "3. Remove a Book\n";
        cout << "4. Update a Book\n";
        cout << "5. Borrow a Book\n";
        cout << "6. Return a Book\n";
        cout << "7. Display All Book\n";
        cout << "8. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        // Process user choice
        switch(choice){
              case 1:
                  addBook(ids, titles, authors, availability, bookCount);
                  break;
              case 2:
                  searchBook(ids,bookCount);
                  break;
              case 3:
                  removeBook(ids, titles, authors, availability, bookCount);
                  break;
              case 4:
                  updateBook(ids, titles, authors,availability, bookCount);
                  break;
              case 5:
                   borrowBook(ids,titles,authors,availability,bookCount);
                  break;
              case 6:
                  returnBook(ids,availability,bookCount);
                  break;
              case 7:
                  displayBooks(ids,titles,authors,availability,bookCount);
                  break;
              case 8:
                  cout << "Exiting the menu. Goodbye!\n";
                  break;
              default:
                cout << "Invalid choice. Please try again.\n";
        }
    }while(choice!=8);
};
int main()
{
    //Arrays of Data
    int currentSize=100;
    int ids[currentSize] = {101, 102, 103};
    string titles[currentSize] = {"C++ Programming", "Data Structures", "Algorithms"};
    string authors[currentSize] = {"Heba", "Basma", "Ibrahim"};
    bool availability[currentSize] = {true, false, true};

    //numbers of current books
    int bookCount = 3;


    menu(ids, titles, authors, availability, bookCount);



    return 0;
}
