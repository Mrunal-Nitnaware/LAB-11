# LAB-11
***To create a binary search tree by given inorder and postorder
#include <stdio.h>
#include <stdlib.h>


typedef struct Node {
    int data;
    struct Node* left;
    struct Node* right;
} Node;


Node* newNode(int data) {
    Node* node = (Node*) malloc(sizeof(Node));
    node->data = data;
    node->left = node->right = NULL;
    return node;
}


int search(int inorder[], int start, int end, int value) {
    for (int i = start; i <= end; i++) {
        if (inorder[i] == value)
            return i;
    }
    return -1;
}


Node* buildTree(int inorder[], int postorder[], int inStart, int inEnd, int* postIndex) {
    if (inStart > inEnd)
        return NULL;

   
    int curr = postorder[*postIndex];
    (*postIndex)--;

   
    Node* node = newNode(curr);

    
    if (inStart == inEnd)
        return node;

   
    int inIndex = search(inorder, inStart, inEnd, curr);

   
    node->right = buildTree(inorder, postorder, inIndex + 1, inEnd, postIndex);
    node->left = buildTree(inorder, postorder, inStart, inIndex - 1, postIndex);

    return node;
}


void printPreorder(Node* node) {
    if (node == NULL)
        return;
    printf("%d ", node->data);
    printPreorder(node->left);
    printPreorder(node->right);
}


int main() {
    int inorder[] = {4, 2, 5, 1, 6, 3};
    int postorder[] = {4, 5, 2, 6, 3, 1};
    int n = sizeof(inorder)/sizeof(inorder[0]);
    
    int postIndex = n - 1;
    Node* root = buildTree(inorder, postorder, 0, n - 1, &postIndex);

    printf("Preorder traversal of the constructed tree:\n");
    printPreorder(root);
    return 0;
}


***Using queue convert 1 to n to binary

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_SIZE 1000
#define MAX_LEN 20


typedef struct {
    char data[MAX_SIZE][MAX_LEN];
    int front, rear;
} Queue;


void initQueue(Queue *q) {
    q->front = q->rear = 0;
}

int isEmpty(Queue *q) {
    return q->front == q->rear;
}

void enqueue(Queue *q, const char *str) {
    strcpy(q->data[q->rear++], str);
}

char* dequeue(Queue *q) {
    return q->data[q->front++];
}


void generateBinary(int n) {
    Queue q;
    initQueue(&q);

    enqueue(&q, "1");

    for (int i = 1; i <= n; i++) {
        char *current = dequeue(&q);
        printf("%s\n", current);

        char left[MAX_LEN], right[MAX_LEN];

       
        strcpy(left, current);
        strcat(left, "0");
        enqueue(&q, left);

       
        strcpy(right, current);
        strcat(right, "1");
        enqueue(&q, right);
    }
}

int main() {
    int n;
    printf("Enter a number (n): ");
    scanf("%d", &n);

    printf("Binary numbers from 1 to %d:\n", n);
    generateBinary(n);

    return 0;
}

