//lab exam question 2

#include <stdio.h>
#include <stdlib.h>

#define QUEUE_SIZE 5

typedef struct {
    int items[QUEUE_SIZE];
    int front;
    int rear;
} CircularQueue;

void initQueue(CircularQueue* q) {
    q->front = -1;
    q->rear = -1;
}

int isFull(CircularQueue* q) {
    if ((q->front == 0 && q->rear == QUEUE_SIZE - 1) || (q->front == q->rear + 1)) {
        return 1;
    }
    return 0;
}

int isEmpty(CircularQueue* q) {
    if (q->front == -1) {
        return 1;
    }
    return 0;
}

void enqueue(CircularQueue* q, int customerID) {
    if (isFull(q)) {
        printf("Queue is full. Please wait.\n");
        return;
    }

    if (isEmpty(q)) {
        q->front = 0;
    }

    q->rear = (q->rear + 1) % QUEUE_SIZE;
    q->items[q->rear] = customerID;
    printf("Call added: %d\n", customerID);
}

int dequeue(CircularQueue* q) {
    if (isEmpty(q)) {
        printf("Queue is empty. No customers to remove.\n");
        return -1;
    }

    int customerID = q->items[q->front];
    printf("Removed customer: %d\n", customerID);

    if (q->front == q->rear) {
        initQueue(q);
    } else {
        q->front = (q->front + 1) % QUEUE_SIZE;
    }
    return customerID;
}

void display(CircularQueue* q) {
    if (isEmpty(q)) {
        printf("Queue is empty.\n");
        return;
    }

    printf("Customers in queue: ");
    int i;
    for (i = q->front; i != q->rear; i = (i + 1) % QUEUE_SIZE) {
        printf("%d ", q->items[i]);
    }
    printf("%d\n", q->items[i]);
}

int main() {
    CircularQueue callQueue;
    initQueue(&callQueue);

    enqueue(&callQueue, 101);
    enqueue(&callQueue, 102);
    enqueue(&callQueue, 103);
    enqueue(&callQueue, 104);
    enqueue(&callQueue, 105);
    display(&callQueue);
    enqueue(&callQueue, 999);
    dequeue(&callQueue);
    dequeue(&callQueue);
    display(&callQueue);
    enqueue(&callQueue, 106);
    enqueue(&callQueue, 107);
    display(&callQueue);

    return 0;
}