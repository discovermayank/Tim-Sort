# Tim-Sort

What is TimSort Algorithm?
TimSort algorithm is a sorting technique widely used in programming. Java and python use this algorithm in their built-in sort() methods. It is a combined hybrid of two other sorting techniques – Insertion-Sort and Merge-Sort

While TimSort is a complex algorithm in itself, where it looks for an ideal minimum size called “minrun”, performs “galloping” in merge-sort to avoid iterations for pre-sorted elements, etc., this post deals with a simple and basic implementation.

It is, however, noteworthy that merge sort is most efficient when the size of the array is a power of 2. Take for instance an array of size 16, which is 2^4. Hence, in each recursion or iteration (depends on the implementation of merge-sort), the array splits into 2 equal subarrays. This happens until we are left with 16 single elements. These are then reconstructed to get the sorted array.

Also, note that insertion sort works best when the size of the array is less. Hence in TimSort, minrun is usually set between 32 and 64. In this implementation, since we are not finding minrun, we are taking 32 as minrun. And from the previous point, we understand that the algorithm is more efficient when (size_of_arr/32) is a power of 2.

TimSort Algorithm
In TimSort, we first start sorting every consecutive set of 32 (i.e minrun) elements of the array using insertion sort.

For instance, if we have an array of size 140, we will have 4 arrays of size 32 and 12 elements remaining. We first perform insertion sort on all these parts, including the one with 12 elements. However, in the complete implementation, 20 more elements will be added to the 12 element subarray and merge sort will be performed. This is because these algorithms first find the most efficient “minrun” value, which improves the overall efficiency.

Let’s call curr_size as 32 initially. We then merge the first 2 sorted subarrays. So now, the first 64 are sorted. Then the next two subarrays are merged. When we are done dividing the array as, sets of 64 continuous sorted elements, we repeat the same process but with two groups of 64 (i.e curr_size = 64) to get 128 and so on till the entire array is sorted. This will happen when curr_size is greater than the sie of the array.

In the complete method involving finding minrun, the remaining elements (12 in this case) will be a number much closer to the chosen minrun. That’s because minrun is chosen based on the most efficient solution. Moreover, it also adds more elements to make up the number to minrun before merging. Since we are not finding minrun here, the last set of elements in each iteration will be a little less efficient. But for the purpose of understanding the core logic of the algorithm, this implementation is sufficient.

The algorithm will be more clear after the programmatic implementation.

InsSort()
This function is used to perform Insertion Sort on the region of the array being passed to function. start is the starting element’s index number and end is the index of the last element of the region. Please refer the post link given at the beginning of this post to understand Insertion Sort. The only deviation from the normal method is the indexes. Instead of starting from 0, we start from start. And similarly, we end with end. The condition in the inner loop becomes j>=start instead of j>=0. The function returns the array after sorting the mentioned subarray in place.

merge()
In this function, we merge the two given subarray indices using the merge sort algorithm. Here again, we start with index start and end with end. Note that the index variable for the main array starts from start and not 0, unlike in the general case. The sizes of the two arrays, first and last are found by finding the difference in the indices. mid is basically the index value of the last element of the subarray – first. the function returns the array after merging the mentioned subarrays in place.

TimSort()
This function acts like the driver function that calls the above-mentioned functions with values pertaining to the logic of TimSort algorithm. We first sort every 32 (i.e minrun) continuous set of elements in the array. That is in an array of size 48, the first 32 elements will be sorted amongst themselves. The next 32 will be looked for, but since there are only 16 left, we simply sort these 16 amongst themselves.

This is what the first for loop does. It finds starting points for each subarray. So for 48 it will assign start = 0 followed by 32 and then exit the loop. end will be assigned 0+32-1 = 31 in the first iteration. In the next iteration, it finds a minimum of (32+32-1) and (48-1). It is 47, hence end=47 in the second iteration.

Boundary Case
Above mentioned is the reason why we sort only the remaining 16 elements in the array. Whenever there are not enough elements to form a complete group of two, we have a condition. It can be of two types:

Let us say we are grouping 2 groups of 32 each. The in the last iteration we might have:

One group with 32 and one more with less than 32 (even 0)
Only one group less than 32.
In case 1, we perform merge sort on the one with 32 and the other one. This boundary case is a little less efficient. But as mentioned earlier, there are ways these can be resolved (determining and efficient minrun and galloping).

In case 2, we leave the group as it is since there is nothing to merge it with and it is already sorted (earlier using insertion sort)

The Process
So now, we have an array with groups of 32 individually sorted elements. Now we set curr_size=32 (minrun initially) and perform merge sort in steps on 2 groups at a time. In the next for loop, 
it first merges the first two groups of 32, the 3rd and 4th and so on. If we come to an odd group i.e one without a pair, it is left as it is. Note the beginning condition in merge() function.

Now, we have groups of 64 individually sorted elements. We repeat the process this time merging two groups of 64 at a time (curr_size=64). 
This results in groups of 128 sorted elements and so on. This process continues as long as curr_size is lesser than the size of the array. 
When this condition becomes false, our array is obviously sorted. This is because, let us say we have 245 elements. When curr_size becomes 256, it means it was 128 in the previous iteration. 
That clearly implies that we have grouped our elements into groups of 256 or less sorted elements. So the 245 elements must be sorted.
