//lab exam question 3

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_STACK_SIZE 10

typedef struct {
    char* items[MAX_STACK_SIZE];
    int top;
} Stack;

Stack* createStack() {
    Stack* stack = (Stack*)malloc(sizeof(Stack));
    if (!stack) {
        perror("Failed to allocate memory for stack");
        exit(EXIT_FAILURE);
    }
    stack->top = -1;
    return stack;
}

int isFull(Stack* stack) {
    return stack->top == MAX_STACK_SIZE - 1;
}

int isEmpty(Stack* stack) {
    return stack->top == -1;
}

void push(Stack* stack, const char* item) {
    if (isFull(stack)) {
        printf("Stack Overflow\n");
        return;
    }
    stack->top++;
    stack->items[stack->top] = strdup(item); 
    if (!stack->items[stack->top]) {
        perror("Failed to allocate memory for stack item");
        exit(EXIT_FAILURE);
    }
}

char* pop(Stack* stack) {
    if (isEmpty(stack)) {
        return NULL;
    }
    char* item = stack->items[stack->top];
    stack->top--;
    return item;
}

char* peek(Stack* stack) {
    if (isEmpty(stack)) {
        return NULL;
    }
    return stack->items[stack->top];
}

void clearStack(Stack* stack) {
    while (!isEmpty(stack)) {
        char* item = pop(stack);
        free(item);
    }
}

void performOperation(Stack* undoStack, Stack* redoStack, const char* operation) {
    printf("Performed operation: \"%s\"\n", operation);
    push(undoStack, operation);
    if (!isEmpty(redoStack)) {
        clearStack(redoStack);
    }
}

void undo(Stack* undoStack, Stack* redoStack) {
    if (isEmpty(undoStack)) {
        printf("Nothing to undo.\n");
        return;
    }
    char* lastOp = pop(undoStack);
    push(redoStack, lastOp);
    
    printf("Undone. ");
    char* nextToUndo = peek(undoStack);
    if (nextToUndo != NULL) {
        printf("Next Operation that can be undone is = \"%s\"\n", nextToUndo);
    } else {
        printf("Undo stack is now empty.\n");
    }
}

void redo(Stack* undoStack, Stack* redoStack) {
    if (isEmpty(redoStack)) {
        printf("Nothing to redo.\n");
        return;
    }
    char* lastUndoneOp = pop(redoStack);
    push(undoStack, lastUndoneOp);
    
    printf("Redo completed. ");
    char* nextToRedo = peek(redoStack);
    if (nextToRedo != NULL) {
        printf("Next Operation that can be redone is = \"%s\"\n", peek(redoStack));
    } else {
        printf("Redo stack is now empty.\n");
    }
}

int main() {
    
    Stack* undoStack = createStack();
    Stack* redoStack = createStack();
    performOperation(undoStack, redoStack, "op1");
    performOperation(undoStack, redoStack, "op2");
    performOperation(undoStack, redoStack, "op3");
    undo(undoStack, redoStack);
    undo(undoStack, redoStack);
    redo(undoStack, redoStack);
    performOperation(undoStack, redoStack, "op4");
    undo(undoStack, redoStack);
    clearStack(undoStack);
    clearStack(redoStack);
    free(undoStack);
    free(redoStack);

    return 0;
}