#include <iostream>
template<class ItemType>
class QueueType {
public:
 QueueType(int max); // Constructor
 ~QueueType(); // Destructor
 void MakeEmpty(); // Method to make the queue empty
 bool IsEmpty() const; // Method to check if the queue is empty
 bool IsFull() const; // Method to check if the queue is full
 void Enqueue(ItemType newItem); // Method to add an item to the queue
 void Dequeue(ItemType& item); // Method to remove an item from the queue
private:
 int front; // Index of the front of the queue
 int rear; // Index of the rear of the queue
 ItemType* items; // Pointer to the array that holds the queue items
 int maxQue; // Maximum number of items in the queue
};
// Constructor
template<class ItemType>
QueueType<ItemType>::QueueType(int max) {
 maxQue = max + 1; // The array size is max + 1 to handle circular behavior
 front = maxQue - 1; // Initialize front
 rear = maxQue - 1; // Initialize rear
 items = new ItemType[maxQue]; // Allocate memory for the queue
}
// Destructor
template<class ItemType>
QueueType<ItemType>::~QueueType() {
 delete[] items; // Deallocate the memory used by the queue
}
// Make the queue empty
template<class ItemType>
void QueueType<ItemType>::MakeEmpty() {
 front = maxQue - 1; // Reset front index
 rear = maxQue - 1; // Reset rear index
}
// Check if the queue is empty
template<class ItemType>
bool QueueType<ItemType>::IsEmpty() const {
 return (rear == front); // The queue is empty if front and rear are equal
}
// Check if the queue is full
template<class ItemType>
bool QueueType<ItemType>::IsFull() const {
 return ((rear + 1) % maxQue == front); // The queue is full if the next position after
rear is front
}
// Add an item to the queue
template<class ItemType>
void QueueType<ItemType>::Enqueue(ItemType newItem) {
 if (IsFull()) {
 throw std::overflow_error("Queue is full");
 }
 rear = (rear + 1) % maxQue; // Move rear to the next position
 items[rear] = newItem; // Add the new item
}
// Remove an item from the queue
template<class ItemType>
void QueueType<ItemType>::Dequeue(ItemType& item) {
 if (IsEmpty()) {
 throw std::underflow_error("Queue is empty");
 }
 front = (front + 1) % maxQue; // Move front to the next position
 item = items[front]; // Retrieve the item
}
// Example usage
int main() {
 QueueType<int> myQueue(5); // Create a queue with a capacity of 5
 myQueue.Enqueue(10);
 myQueue.Enqueue(20);
 myQueue.Enqueue(30);
 int item;
 myQueue.Dequeue(item);
 std::cout << "Dequeued item: " << item << std::endl;
 myQueue.Dequeue(item);
 std::cout << "Dequeued item: " << item << std::endl;
 return 0;
}        

































#include <iostream>
using namespace std;
// Forward declaration of NodeType
template<class ItemType>
struct NodeType {
 ItemType info;
 NodeType<ItemType>* next;
};
// QueueType class definition
template<class ItemType>
class QueueType {
public:
 QueueType();
 ~QueueType();
 void MakeEmpty();
 bool IsEmpty() const;
 bool IsFull() const;
 void Enqueue(ItemType newItem);
 void Dequeue(ItemType& item);
private:
 NodeType<ItemType>* qFront;
 NodeType<ItemType>* qRear;
};
// Constructor
template<class ItemType>
QueueType<ItemType>::QueueType() {
 qFront = nullptr;
 qRear = nullptr;
}
// Destructor
template<class ItemType>
QueueType<ItemType>::~QueueType() {
 MakeEmpty();
}
// Make the queue empty
template<class ItemType>
void QueueType<ItemType>::MakeEmpty() {
 NodeType<ItemType>* tempPtr;

 while (qFront != nullptr) {
 tempPtr = qFront;
 qFront = qFront->next;
 delete tempPtr;
 }
 qRear = nullptr;
}
// Check if the queue is empty
template<class ItemType>
bool QueueType<ItemType>::IsEmpty() const {
 return (qFront == nullptr);
}
// Check if the queue is full (if memory allocation fails)
template<class ItemType>
bool QueueType<ItemType>::IsFull() const {
 NodeType<ItemType>* ptr = new NodeType<ItemType>;
 if (ptr == nullptr)
 return true;
 else {
 delete ptr;
 return false;
 }
}
// Enqueue a new item to the queue
template<class ItemType>
void QueueType<ItemType>::Enqueue(ItemType newItem) {
 NodeType<ItemType>* newNode = new NodeType<ItemType>;
 newNode->info = newItem;
 newNode->next = nullptr;
 if (qRear == nullptr)
 qFront = newNode;
 else
 qRear->next = newNode;
 qRear = newNode;
}
// Dequeue an item from the queue
template<class ItemType>
void QueueType<ItemType>::Dequeue(ItemType& item){
 NodeType<ItemType>* tempPtr;
 tempPtr = qFront;
 item = qFront->info;
 qFront = qFront->next;
 if (qFront == nullptr)
 qRear = nullptr;
 delete tempPtr;
}
// Main function to test the QueueType class
int main() {
 QueueType<int> q;
 // Test Enqueue
 q.Enqueue(10);
 q.Enqueue(20);
 q.Enqueue(30);
 // Test Dequeue
 int item;
 q.Dequeue(item);
 cout << "Dequeued: " << item << endl; // Should print 10
 q.Dequeue(item);
 cout << "Dequeued: " << item <<endl; // Should print 20
 q.Dequeue(item);
 cout << "Dequeued: " << item << endl; // Should print 30
 // Test IsEmpty
 if (q.IsEmpty()) {
 cout << "Queue is empty." <<endl;
 } else {
 cout << "Queue is not empty." << endl;
 }
 return 0;}
