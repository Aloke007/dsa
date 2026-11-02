C
// 1. Linear Search
#include <stdio.h>
int linearSearch(int arr[], int n, int key) {
    for (int i = 0; i < n; i++) if (arr[i] == key) return i;
    return -1;
}

// 1. Binary Search
int binarySearch(int arr[], int n, int key) {
    int low = 0, high = n - 1;
    while (low <= high) {
        int mid = (low + high) / 2;
        if (arr[mid] == key) return mid;
        if (key < arr[mid]) high = mid - 1;
        else low = mid + 1;
    }
    return -1;
}

// 2. Bubble Sort
void bubbleSort(float arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                float temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}

// 2. Selection Sort
void selectionSort(float arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int min_idx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[min_idx]) min_idx = j;
        }
        float temp = arr[i];
        arr[i] = arr[min_idx];
        arr[min_idx] = temp;
    }
}

// 3. Hash Table - Linear Probing
#define SIZE 10
void insertLinear(int table[], int key) {
    for (int i = 0; i < SIZE; i++) {
        int probe = (key % SIZE + i) % SIZE;
        if (table[probe] == -1) { table[probe] = key; return; }
    }
}

// 3. Hash Table - Double Hashing
void insertDouble(int table[], int key) {
    int step = 7 - (key % 7);
    for (int i = 0; i < SIZE; i++) {
        int probe = (key % SIZE + i * step) % SIZE;
        if (table[probe] == -1) { table[probe] = key; return; }
    }
}

// 4. Singly Linked List (Pinnacle Club)
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct Node {
    int prn;
    char name[50];
    struct Node* next;
} Node;

Node* createNode(int prn, const char* name) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->prn = prn;
    strcpy(newNode->name, name);
    newNode->next = NULL;
    return newNode;
}

Node* addPresident(Node* head, int prn, const char* name) {
    Node* newNode = createNode(prn, name);
    newNode->next = head;
    return newNode;
}

Node* addSecretary(Node* head, int prn, const char* name) {
    Node* newNode = createNode(prn, name);
    if (head == NULL) return newNode;
    Node* temp = head;
    while (temp->next != NULL) temp = temp->next;
    temp->next = newNode;
    return head;
}

Node* deleteMember(Node* head, int prn) {
    if (head == NULL) return NULL;
    Node* temp = head; 
    Node* prev = NULL;
    while (temp != NULL && temp->prn != prn) { 
        prev = temp; 
        temp = temp->next; 
    }
    if (temp == NULL) return head;
    if (prev != NULL) prev->next = temp->next;
    else head = temp->next;
    free(temp); 
    return head;
}

// 5. Circular Doubly Linked List (Cinemax Booking)
#include <stdio.h>
#include <stdlib.h>

#define ROWS 10
#define SEATS 7

typedef struct Seat {
    int seatNo; 
    int isBooked;
    struct Seat *prev, *next;
} Seat;

Seat* headRow[ROWS];

void initTheater() {
    for (int r = 0; r < ROWS; r++) {
        Seat *head = NULL, *tail = NULL;
        for (int i = 1; i <= SEATS; i++) {
            Seat* newSeat = (Seat*)malloc(sizeof(Seat));
            newSeat->seatNo = i; 
            newSeat->isBooked = 0;
            if (head == NULL) { 
                head = tail = newSeat; 
                head->next = head->prev = head; 
            } else {
                tail->next = newSeat; 
                newSeat->prev = tail;
                newSeat->next = head; 
                head->prev = newSeat; 
                tail = newSeat;
            }
        }
        headRow[r] = head;
    }
}

void bookSeat(int row, int seatNum) {
    Seat* temp = headRow[row - 1];
    do {
        if (temp->seatNo == seatNum) {
            if(temp->isBooked) printf("Seat Already Booked!\n");
            else { temp->isBooked = 1; printf("Seat Booked.\n"); }
            return;
        }
        temp = temp->next;
    } while (temp != headRow[row - 1]);
}

// 6. Stack - Infix to Postfix & Evaluation (Core Logic)
#include <ctype.h>
char cStack[100]; int cTop = -1;
void pushC(char c) { cStack[++cTop] = c; }
char popC() { return cStack[cTop--]; }
int pre(char op) { return (op=='^')?3 : (op=='*'||op=='/')?2 : (op=='+'||op=='-')?1 : 0; }
void infixToPostfix(char in[], char out[]) {
    int k = 0; cTop = -1;
    for(int i = 0; in[i]; i++) {
        if(isalnum(in[i])) out[k++] = in[i];
        else if(in[i] == '(') pushC('(');
        else if(in[i] == ')') { while(cStack[cTop] != '(') out[k++] = popC(); popC(); }
        else { 
            while(cTop != -1 && cStack[cTop] != '(' && pre(in[i]) <= pre(cStack[cTop]))
                out[k++] = popC();
            pushC(in[i]);
        }
    }
    while(cTop != -1) out[k++] = popC();
    out[k] = '\0';
}

// 7. Circular Queue
#define MAX 5
int Q[MAX], front = -1, rear = -1;
void enqueue(int val) {
    if ((rear + 1) % MAX == front) return;
    if (front == -1) front = 0;
    rear = (rear + 1) % MAX;
    Q[rear] = val;
}
int dequeue() {
    if (front == -1) return -1;
    int val = Q[front];
    if (front == rear) { front = rear = -1; }
    else front = (front + 1) % MAX;
    return val;
}

// 8. Binary Search Tree (BST)
#include <stdio.h>
#include <stdlib.h>

typedef struct BSTNode {
    int data;
    struct BSTNode *left, *right;
} BSTNode;

BSTNode* createNodeBST(int value) {
    BSTNode* newNode = (BSTNode*)malloc(sizeof(BSTNode));
    newNode->data = value; 
    newNode->left = newNode->right = NULL;
    return newNode;
}

BSTNode* insertBST(BSTNode* root, int value) {
    if (root == NULL) return createNodeBST(value);
    if (value < root->data) root->left = insertBST(root->left, value);
    else if (value > root->data) root->right = insertBST(root->right, value);
    return root;
}

void mirrorTree(BSTNode* root) {
    if (root == NULL) return;
    mirrorTree(root->left); 
    mirrorTree(root->right);
    BSTNode* temp = root->left; 
    root->left = root->right; 
    root->right = temp;
}

int findHeight(BSTNode* root) {
    if (root == NULL) return 0;
    int lh = findHeight(root->left);
    int rh = findHeight(root->right);
    return (lh > rh ? lh : rh) + 1;
}

// 9. Graph - DFS Connectivity
#include <stdio.h>
#include <stdlib.h>

#define MAX_CITIES 20

typedef struct FlightNode {
    int destIndex; 
    int cost;
    struct FlightNode* next;
} FlightNode;

typedef struct {
    int numCities;
    FlightNode* adjList[MAX_CITIES];
} FlightGraph;

int visited[MAX_CITIES];

void addFlight(FlightGraph* g, int src, int dest, int cost) {
    FlightNode* newNode = (FlightNode*)malloc(sizeof(FlightNode));
    newNode->destIndex = dest; 
    newNode->cost = cost; 
    newNode->next = g->adjList[src]; 
    g->adjList[src] = newNode;
    
    // For undirected flight paths
    FlightNode* revNode = (FlightNode*)malloc(sizeof(FlightNode));
    revNode->destIndex = src; 
    revNode->cost = cost; 
    revNode->next = g->adjList[dest]; 
    g->adjList[dest] = revNode;
}

void DFS(FlightGraph* g, int u) {
    visited[u] = 1;
    FlightNode* temp = g->adjList[u];
    while (temp != NULL) {
        if (!visited[temp->destIndex]) {
            DFS(g, temp->destIndex);
        }
        temp = temp->next;
    }
}

int isConnected(FlightGraph* g) {
    for (int i = 0; i < g->numCities; i++) visited[i] = 0;
    
    DFS(g, 0); // Start traversal from node 0
    
    for (int i = 0; i < g->numCities; i++) {
        if (!visited[i]) return 0; // Graph is disconnected
    }
    return 1; // Graph is connected
}
