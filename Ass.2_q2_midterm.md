//midterm question 2

#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int songID;
    struct Node* next;
} Node;

Node* createNode(int songID) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->songID = songID;
    newNode->next = NULL;
    return newNode;
}

void printList(Node* head) {
    Node* temp = head;
    while (temp != NULL) {
        printf("%d", temp->songID);
        if (temp->next != NULL)
            printf(" -> ");
        temp = temp->next;
    }
    printf("\n");
}

Node* reversePlaylistSegment(Node* head, int m, int n) {
    if (!head || m == n) return head;

    Node dummy;
    dummy.next = head;
    Node* prev = &dummy;

    for (int i = 1; i < m; i++) {
        if (!prev) return head;
        prev = prev->next;
    }

    Node* curr = prev->next;
    Node* next = NULL;
    Node* tail = curr; 

    for (int i = m; i < n && curr && curr->next; i++) {
        next = curr->next;
        curr->next = next->next;
        next->next = prev->next;
        prev->next = next;
    }

    return dummy.next;
}

int main() {

    Node* head = createNode(101);
    head->next = createNode(102);
    head->next->next = createNode(103);
    head->next->next->next = createNode(104);
    head->next->next->next->next = createNode(105);
    head->next->next->next->next->next = createNode(106);
    head->next->next->next->next->next->next = createNode(107);

    printf("Original Playlist:\n");
    printList(head);

    int m = 2, n = 5;
    head = reversePlaylistSegment(head, m, n);

    printf("\nModified Playlist (after reversing from %d to %d):\n", m, n);
    printList(head);

    return 0;
}
