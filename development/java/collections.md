# Collections

## Comparator

`int compare(T o1, T o2)` returns: a negative integer, zero, or a positive integer as the first argument is less than, equal to, or greater than the second.

## Comparable

`int compareTo(T o)` returns: a negative integer, zero, or a positive integer as this object is less than, equal to, or greater than the specified object.

## Arrays

Binary search requires input to be sorted:

```text
int arr[] = new int[] {4,3,2,1};
Arrays.sort(arr);
System.out.println(Arrays.binarySearch(arr, 2)); // 1
```

When element missing binary search returns predicted position in array where it can be. The exact result follows the next formula: _negative possible position in the array - 1_:

```text
int arr[] = new int[] {1,3,5,7,9};
System.out.println(Arrays.binarySearch(arr, 4));// -3 -> (-2 - 1)
```

`compare` does lexicographical comparison:

```text
int arr[] = new int[] {0,3};
int arr2[] = new int[] {1,2};
System.out.println(Arrays.compare(arr, arr2)); // -1

String arr3[] = new String[] {"a"};
String arr4[] = new String[] {"c"};
System.out.println(Arrays.compare(arr3, arr4)); // -2 -> (a-c)
```

## Set

`copyOf(collection)` returns and immutable set with collection copy inside.

`TreeSet` does not allow null values as it needs to compare them.

## List

`copyOf(collection)` returns and immutable list with collection copy inside.

## Queue

`offer` adds to the end;  
`poll` retrieves and removes the head;  
`peek` retrieves but not removes the head;

## Deque

`offer` adds to the end;  
`poll` retrieves and removes the head;  
`peek` retrieves but not removes the head;  
`push == addFirst`;  
`pop == removeFirst`;

