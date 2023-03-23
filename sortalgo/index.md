# Sort Algorithm


----

Sort algorithm (yshenn acquired)

----

***Today is 2023.3.21***  
In the past week, I've learned some sort algorithms. They do not seem as complex as i imagined. So I make a conclusion here.

### sort algorithm list
- Insertion Sort
- Selection Sort
- Bubble Sort
- Merge Sort
- Heap Sort
- Quick Sort


### Details

Insertion Sort
```c++
void insertionSort(vector<int> &arr)
{
	int size = (int)arr.size();
	for (int i = 1; i < size; i++) {
		int j = i - 1;
		int key = arr[i];
		while (j > -1 && arr[j] > key) {
			arr[j+1] = arr[j];
			j--;
		}
		arr[j+1] = key;
	}
}
```

Selection Sort
```c++
void selectionSort(vector<int> &arr)
{
	int size = (int)arr.size();
	for (int i = 0; i < size-1; i++) {
		int key = arr[i];
		int position = i;
		for (int j = i+1; j < size; j++) {
			if (arr[j] < key) {
				key = arr[j];
				position = j;
			}
		}
		arr[position] = arr[i];
		arr[i] = key;
	}
}
```

Bubble Sort
```c++
void bubbleSort(vector<int> &arr)
{
	int size = (int)arr.size();
	for (int i = 0; i < size-1; i++) {
		for(int j = 0; j < size-i-1; j++) {
			if (arr[j] > arr[j+1]) {
				int tmp = arr[j];
				arr[j] = arr[j+1];
				arr[j+1] = tmp;
			}
		}
	}
}
```

Merge Sort
```c++
//#include<limits>
//to use "numeric_limits<int>::max()" role as the infinite

void merge(vector<int> &arr, int p, int q, int r)
{
	int llen = q - p + 1;
	int rlen = r - q;
	vector<int> lsub(llen+1);
	vector<int> rlen(rlen+1);
	for (int i = 0; i < llen; i++)
		lsub[i] = arr[p+i];
	for (int i = 0; i < rlen; i++)
		rsub[i] = arr[q+i+1];
	lsub[llen] = numeric_limits<int>::max();
	rsub[rlen] = numeric_limits<int>::max();
	int i = 0, j = 0;
	for (int k = p; k <= r; k++) {
		if (lsub[i] < rsub[j]) {
			arr[k] = lsub[i];
			i++;
		}
		else {
			arr[k] = rsub[j];
			j++;
		}
	}
}

void mergeSort(vector<int> &arr, int p, int r)
{
	if (p < r) {
		int q = (p + r) / 2;
		mergeSort(arr, p, q);
		mergeSort(arr, q+1, r);
		merge(arr, p, q, r);
	}
	
}
```
Heap Sort
```c++
#define LEFT (i<<1)     //to get the left child position (2*i)
#define RIGHT ((i<<1)+1)//to get the right child position (2*i+1)

void maxHeapify(vector<int> &arr, int i, int heap_size)
{
	int largest = i;
	int l = LEFT(i);
	int r = RIGHT(i);
	if (l < heap_size && arr[l-1] > arr[i-1])
		largest = l;
	else
		largest = i;
	if (r < heap_size && arr[r-1] > arr[largest-1])
		largest = r;
	if (largest != i) {
		int tmp = arr[i-1];
		arr[i-1] = arr[largest-1];
		arr[largest-1] = tmp;
		maxHeapify(arr, largest, heap_size);
	}
}

void buildMaxHeap(vector<int> &arr)
{
	int size = (int)arr.size();
	for (int i = (size/2); i > 0; i--) {
		maxHeapify(arr, i, size);
	}
}

void heapSort(vector<int> &arr)
{
	buildMaxHeap(arr);
	int size = (int)arr.size();
	int tmp = 0;
	while (size > 1) {
		tmp = arr[0];
		arr[0] = arr[size-1];
		arr[size-1] = tmp;
		size--;
		maxHeapify(arr, 1, size);
	}
}
```

Quick Sort
```c++
int partition(vector<int> &arr, int p, int r)
{
	int pivot = arr[r];
	int i = p - 1;
	int tmp = 0;
	for (int j = p; j < r; j++) {
		if (arr[j] < pivot) {
			i++;
			tmp = arr[i];
			arr[i] = arr[j];
			arr[j] = tmp;
		}
	}
	arr[r] = arr[i+1];
	arr[i+1] = pivot;

	return i+1;
}

void quickSort(vector<int> &arr, int p, int r)
{
	if (p < r) {
		int q = partition(arr, p, r);
		quickSort(arr, p, q-1);
		quickSort(arr, q+1, r);
	}
}
```

### Test
I use `sort_test.h` and `minunit.h` to test these sort algorithms, bringing me great convenience

In the headerfile `sort_test.h`, the main api is function `tests()`. Just specify sort algorithm's name as the argument passed into it, if the algorithm works well, you will get its running time. Thus, you can get the efficiency of different sort algorithm easily.
```c++
//sort_test.h

#ifndef _SORT_TEST_H
#define _SORT_TEST_H

#include <bits/types/clock_t.h>
#include <gtest/gtest.h>
#include <iostream>
#include <vector>
#include <cstdlib>
#include <algorithm>
#include<ctime>
#include "minunit.h"

#define TEST_TIMES 100000

using namespace std;

void insertionSort(vector<int>& arr);
void selectionSort(vector<int>& arr);
void bubbleSort(vector<int>& arr);
void mergeSort(vector<int> &arr, int p , int r);
void heapSort(vector<int>& arr);
void quickSort(vector<int>& arr, int p, int r);

static void randomCases(vector<int> &arr)
{
	srand((int)time(0));	
	//srand(1);	
	int size = rand()%1000 + 1;
	//int size = 100;
	for (int i = 0; i < size; i++)
		arr.push_back(rand()%1000);
}

static string testSort(string sortName)
{
	vector<int> arr;
	randomCases(arr);
	int size = (int)arr.size();
	vector<int> arrSorted = arr;
	sort(arrSorted.begin(), arrSorted.end());
	if (sortName == "insertionSort")
		insertionSort(arr);
	else if (sortName == "selectionSort")
		selectionSort(arr);
	else if (sortName == "bubbleSort")
		bubbleSort(arr);
	else if (sortName == "mergeSort")
		mergeSort(arr, 0, size-1);
	else if (sortName == "heapSort")
		heapSort(arr);
	else if (sortName == "quickSort")
		quickSort(arr, 0, size-1);
	else 
		mu_assert("ERROR:Can not find sort algorithm specified", 0);
	for (int i = 0; i < size; i++) {
		if (arr[i] != arrSorted[i])
			mu_assert("ERROR:Sort failed", 0);
	}
	return "TEST WELL";
}

static void tests(string sortName)
{
	string message;

	clock_t start, end;
	start = clock();
	for (int i = 0; i < TEST_TIMES; i++)
		message = testSort(sortName);
	end = clock();
	
	clock_t rtime = end - start;
	clock_t avgtime = rtime/TEST_TIMES;
	cout << sortName<< " : "<< message << endl;
	cout << "Running time: " << rtime/CLOCKS_PER_SEC << "s"<< endl; 
	cout << "Average running time: " << avgtime/CLOCKS_PER_SEC << "s"<< endl;
}

#endif
```

The headfile `minunit.h` is just almost the minimal unit test frame, I got it from [here](https://jera.com/techinfo/jtns/jtn002);
```c++
//minunit.h

#define mu_assert(message, test) do { if (!(test)) return message;} while (0)
#define mu_run_test(test) do { char *message = test(); tests_run++; \
						  if (message) return message;} while(0)
extern int tests_run;

```

### TODO
- ***use loop invariant to proof the correctness of algorithm***  
- ***to analyze the complexity of algorithm***











