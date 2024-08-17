# Partition DP

## Format
A linear data structure will be given and we would have to divide it into multiple parts to get some condition satisfied. We would take a segment [i.....j] such that, i<=j and divide this segment into parts like [i.....k] and [k....j] such that, i<=k<=j, i.e., k would vary between i and j and we would solve the problem for those sub-segments and use the temporary answer got from it to calculate the answer for the segment [i.....j] and hence for the complete length of the given linear data structure.
