//mid term question 3

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_QUEUE_SIZE 100

typedef struct Node {
    char data;
    int frequency;
    struct Node *left;
    struct Node *right;
} Node;

typedef struct Queue {
    Node* items[MAX_QUEUE_SIZE];
    int front;
    int rear;
} Queue;

Node* createNode(char data) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->data = data;
    newNode->frequency = 1;
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}

Queue* createQueue() {
    Queue* q = (Queue*)malloc(sizeof(Queue));
    q->front = -1;
    q->rear = -1;
    return q;
}

int isEmpty(Queue* q) {
    return q->rear == -1;
}

void enqueue(Queue* q, Node* node) {
    if (q->rear == MAX_QUEUE_SIZE - 1) {
        printf("Queue is full\n");
        return;
    }
    if (q->front == -1) {
        q->front = 0;
    }
    q->rear++;
    q->items[q->rear] = node;
}

Node* dequeue(Queue* q) {
    if (isEmpty(q)) {
        printf("Queue is empty\n");
        return NULL;
    }
    Node* item = q->items[q->front];
    q->front++;
    if (q->front > q->rear) {
        q->front = q->rear = -1;
    }
    return item;
}

Node* buildTree(const char* str) {
    if (str == NULL || str[0] == '\0') {
        return NULL;
    }

    Node* root = NULL;
    Node* char_map[256] = {NULL};
    Queue* q = createQueue();

    for (int i = 0; str[i] != '\0'; i++) {
        char currentChar = str[i];
        if (char_map[(int)currentChar] != NULL) {
            char_map[(int)currentChar]->frequency++;
        } else {
            Node* newNode = createNode(currentChar);
            char_map[(int)currentChar] = newNode;

            if (root == NULL) {
                root = newNode;
            } else {
                Node* parent = q->items[q->front];
                if (parent->left == NULL) {
                    parent->left = newNode;
                } else {
                    parent->right = newNode;
                    dequeue(q);
                }
            }
            enqueue(q, newNode);
        }
    }

    free(q);
    return root;
}

void levelOrderTraversal(Node* root) {
    if (root == NULL) {
        printf("Tree is empty.\n");
        return;
    }

    Queue* q = createQueue();
    enqueue(q, root);
    int isFirst = 1;

    while (!isEmpty(q)) {
        Node* current = dequeue(q);
        if (!isFirst) {
            printf(", ");
        }
        printf("(%c,%d)", current->data, current->frequency);
        isFirst = 0;

        if (current->left != NULL) {
            enqueue(q, current->left);
        }
        if (current->right != NULL) {
            enqueue(q, current->right);
        }
    }
    printf("\n");
    free(q);
}

int main() {
    char input[] = "programming";
    printf("Input String: \"%s\"\n\n", input);

    Node* root = buildTree(input);

    printf("Level Order Traversal: ");
    levelOrderTraversal(root);

    return 0;
}