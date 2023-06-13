# Movies-tickets-booking-system
 How to make the Movies tickets booking system  using c progarmming
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
// Function to display the current movie
void display_movie() {
    printf("\nCurrent movie: Avengers: Endgame\n");
}
// Structure for storing user details
struct user {
    char name[60];
    char email[50];
    char phone[15];
};
// Function for user registration
void register_user(struct user* u) {
    printf("\nEnter Details:\n");
    fgets(u->name, sizeof(u->name), stdin); 
    printf("Enter your email: ");
    fgets(u->email, sizeof(u->email), stdin);
    printf("Enter your phone number: ");
    fgets(u->phone, sizeof(u->phone), stdin);
    printf("Registration successful.\n");
}
// Structure for storing booking details
struct booking {
    int id;
    struct user u;
    int no_of_tickets;
    float amount_paid;
};
// Array of booking structures to store booking records
struct booking bookings[100];
int num_bookings = 0;

// Function for booking tickets
void book_ticket() {
    // Check if maximum number of bookings has been reached
    if (num_bookings >= 100) {
        printf("\nSorry, maximum number of bookings reached.\n");
        return;
    }
    struct booking b;
    b.id = num_bookings + 1;
    printf("\nBooking ID: %d\n", b.id);
    register_user(&b.u);
    printf("Enter number of tickets: ");
    scanf("%d", &b.no_of_tickets);
    printf("Enter amount paid: ");
    scanf("%f", &b.amount_paid);
    bookings[num_bookings++] = b;
    printf("Booking successful.\n");

    // Save booking details to file
    FILE* fp;
    fp = fopen("bookings.txt", "a");
    if (fp != NULL) {
        fprintf(fp, "%d\t%s\t%s\t%s\t%d\t%.2f\n", b.id, b.u.name, b.u.email, b.u.phone, b.no_of_tickets, b.amount_paid);
        fclose(fp);
    } else {
        printf("Error: Unable to open file for saving booking details.\n");
    }
}

// Function for canceling tickets
void cancel_ticket() {
    int id;
    printf("\nEnter booking ID to cancel: ");
    scanf("%d", &id);
    if (id < 1 || id > num_bookings) {
        printf("Invalid booking ID.\n");
        return;
    }
    for (int i = id - 1; i < num_bookings - 1; i++) {
        bookings[i] = bookings[i + 1];
    }
    num_bookings--;
    printf("Booking with ID %d canceled.\n", id);
    
    // Update booking details file
    FILE* fp;
    fp = fopen("bookings.txt", "w");
    if (fp != NULL) {
        for (int i = 0; i < num_bookings; i++) {
            fprintf(fp, "%d\t%s\t%s\t%s\t%d\t%.2f\n", bookings[i].id, bookings[i].u.name, bookings[i].u.email, bookings[i].u.phone, bookings[i].no_of_tickets, bookings[i].amount_paid);
        }
        fclose(fp);
        
    } else {
        printf("Error: Unable to open file for updating booking details.\n");
    }
}
// Function to view all booking records
void view_bookings() {
    if (num_bookings == 0) {
        printf("\nNo bookings found.\n");
        return;
    }
    printf("\nBooking ID\tUser Name\tEmail\t\tPhone\t\tNo. of tickets\tAmount paid\n");
    for (int i = 0; i < num_bookings; i++) {
        printf("%d\t\t%s\t%s\t%s\t%d\t\t%.2f\n", bookings[i].id, bookings[i].u.name, bookings[i].u.email, bookings[i].u.phone, bookings[i].no_of_tickets, bookings[i].amount_paid);
    }
}
// Function for payment options
void payment_options() {
    printf("\nPayment options: Credit card, Debit card, Net banking\n");
}
int main() {
    int choice;
    do {
        printf("\nMOVIE TICKET BOOKING SYSTEM\n");
        printf("1. Display current movie\n");
        printf("2. Book ticket\n");
        printf("3. Cancel ticket\n");
        printf("4. View all booking records\n");
        printf("5. Payment options\n");
        printf("6. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        switch (choice){
		
                case 1:
            display_movie();
            break;
        case 2:
            book_ticket();
            break;
        case 3:
            cancel_ticket();
            break;
        case 4:
            view_bookings();
            break;
        case 5:
            payment_options();
            break;
        case 6:
            printf("\nThank you for using our system!\n");
            exit(0);
        default:
            printf("\nInvalid choice. Please try again.\n");
        }
    } while (1);
    return 0;
}
