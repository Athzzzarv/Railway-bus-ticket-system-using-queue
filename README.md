# Railway-bus-ticket-system-using-queue


# Railway/Bus Ticket Waiting System using Queue in C

## 1. Introduction

The Railway/Bus Ticket Waiting System is a console-based application developed using the C programming language. It uses the Queue data structure to manage ticket bookings and waiting lists.

When all seats are occupied, new passengers are placed in a waiting queue. If a booked ticket is cancelled, the first passenger in the waiting queue is automatically assigned the vacant seat.

The system follows the FIFO (First In, First Out) principle.

## 2. Objectives

- Understand and implement the Queue data structure.
- Demonstrate the FIFO principle.
- Manage ticket bookings and cancellations.
- Maintain a passenger waiting list.
- Automatically allocate cancelled seats to waiting passengers.
- Apply arrays, functions, loops, and conditional statements in C.

## 3. Features

- Book tickets for passengers.
- Automatically assign available seats.
- Add passengers to a waiting list when all seats are occupied.
- Cancel booked tickets.
- Automatically allocate cancelled seats to the first waiting passenger.
- Display all booked passengers.
- Display the waiting list.
- Handle invalid seat numbers.
- Detect a full waiting list.

## 4. Data Structures Used

| Data Structure | Purpose |
|---------------|---------|
| Array | Stores booked passenger information |
| Circular Queue | Stores waiting passengers |
| Front | Points to the first passenger in the queue |
| Count | Tracks the number of waiting passengers |
| Rear | Calculated during insertion to add passengers at the end |

## 5. Queue Principle

The system follows FIFO (First In, First Out).

Example:

Waiting Queue:

FRONT -> Passenger A -> Passenger B -> Passenger C <- REAR

When a seat becomes available:

Passenger A receives the seat.

Updated Queue:

FRONT -> Passenger B -> Passenger C <- REAR

Passenger A is removed because they were the first passenger to enter the waiting queue.

## 6. Algorithm

### Ticket Booking

1. Start.
2. Accept the passenger's name.
3. Check whether seats are available.
4. If a seat is available, assign the passenger to that seat.
5. If all seats are occupied, add the passenger to the waiting queue.
6. Display the booking status.
7. Stop.

### Ticket Cancellation

1. Start.
2. Accept the seat number to be cancelled.
3. Validate the seat number.
4. Check whether the seat is occupied.
5. Cancel the booking.
6. If the waiting queue is not empty:
   - Assign the vacant seat to the passenger at the front.
   - Remove that passenger from the queue.
7. Otherwise, mark the seat as vacant.
8. Display the cancellation status.
9. Stop.

### Display Waiting List

1. Start.
2. Check whether the waiting queue is empty.
3. If empty, display an appropriate message.
4. Otherwise, traverse the queue from front to rear.
5. Display all waiting passengers.
6. Stop.

## 7. Source Code

```c
#include <stdio.h>
#include <string.h>

#define MAX_SEATS 5
#define MAX_WAIT 50
#define NAME_LEN 50

char seats[MAX_SEATS][NAME_LEN];

char waiting[MAX_WAIT][NAME_LEN];
int front = 0, count = 0;

void bookTicket()
{
    char name[NAME_LEN];
    int i;

    printf("Enter passenger name: ");
    scanf(" %49[^\n]", name);

    // Check for available seats
    for (i = 0; i < MAX_SEATS; i++)
    {
        if (seats[i][0] == '\0')
        {
            strcpy(seats[i], name);
            printf("\nTicket booked successfully!\n");
            printf("Seat number: %d\n", i + 1);
            return;
        }
    }

    // No seats available, add to waiting queue
    if (count == MAX_WAIT)
    {
        printf("\nWaiting list is full!\n");
        return;
    }

    int rear = (front + count) % MAX_WAIT;
    strcpy(waiting[rear], name);
    count++;

    printf("\nAll seats are booked.\n");
    printf("Passenger added to waiting list.\n");
}

void cancelTicket()
{
    int seatNo;

    printf("Enter seat number to cancel (1-%d): ", MAX_SEATS);
    scanf("%d", &seatNo);

    if (seatNo < 1 || seatNo > MAX_SEATS)
    {
        printf("Invalid seat number!\n");
        return;
    }

    if (seats[seatNo - 1][0] == '\0')
    {
        printf("This seat is already vacant!\n");
        return;
    }

    printf("Ticket cancelled for %s\n", seats[seatNo - 1]);

    // If waiting passengers exist, assign the seat
    if (count > 0)
    {
        strcpy(seats[seatNo - 1], waiting[front]);

        printf("%s from the waiting list has been allotted seat %d.\n",
               waiting[front], seatNo);

        front = (front + 1) % MAX_WAIT;
        count--;
    }
    else
    {
        seats[seatNo - 1][0] = '\0';
        printf("Seat is now available.\n");
    }
}

void displayBooked()
{
    printf("\n--- BOOKED PASSENGERS ---\n");

    for (int i = 0; i < MAX_SEATS; i++)
    {
        if (seats[i][0] != '\0')
            printf("Seat %d : %s\n", i + 1, seats[i]);
        else
            printf("Seat %d : Vacant\n", i + 1);
    }
}

void displayWaiting()
{
    printf("\n--- WAITING LIST ---\n");

    if (count == 0)
    {
        printf("Waiting list is empty.\n");
        return;
    }

    for (int i = 0; i < count; i++)
    {
        int index = (front + i) % MAX_WAIT;
        printf("%d. %s\n", i + 1, waiting[index]);
    }
}

int main()
{
    int choice;

    // Initialize seats as vacant
    for (int i = 0; i < MAX_SEATS; i++)
        seats[i][0] = '\0';

    do
    {
        printf("\n===== RAILWAY / BUS TICKET SYSTEM =====\n");
        printf("1. Book Ticket\n");
        printf("2. Cancel Ticket\n");
        printf("3. Display Booked Passengers\n");
        printf("4. Display Waiting List\n");
        printf("5. Exit\n");

        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice)
        {
            case 1:
                bookTicket();
                break;

            case 2:
                cancelTicket();
                break;

            case 3:
                displayBooked();
                break;

            case 4:
                displayWaiting();
                break;

            case 5:
                printf("Thank you for using the system!\n");
                break;

            default:
                printf("Invalid choice! Try again.\n");
        }

    } while (choice != 5);

    return 0;
}
