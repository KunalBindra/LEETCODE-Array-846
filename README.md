# LEETCODE-Array-846
Let's do a dry run of the `isNStraightHand` method from the `Solution` class to understand how it works.

The `isNStraightHand` method takes two parameters:
- `hand`: an array of integers representing the cards.
- `groupSize`: the size of the groups we need to form with consecutive cards.

The method returns `true` if we can rearrange the cards to form groups of `groupSize` consecutive cards, and `false` otherwise.

Here's the step-by-step breakdown:

### Initialization

1. **Create a `TreeMap` called `count`**:
   - This will keep track of the frequency of each card in `hand`.

```java
TreeMap<Integer, Integer> count = new TreeMap<>();
```

### Populate the `TreeMap`

2. **Count the frequency of each card in `hand`**:
   - Iterate over the `hand` array and use `count.merge(card, 1, Integer::sum)` to update the frequency of each card.

```java
for (final int card : hand)
  count.merge(card, 1, Integer::sum);
```

### Check and Form Groups

3. **Check each card and try to form groups**:
   - Iterate over the keys of `count` (the card values in sorted order).
   - For each card (denoted as `start`), check if there are any cards left to process (`value > 0`).
   - Try to form a group of `groupSize` consecutive cards starting from `start`.

```java
for (final int start : count.keySet()) {
  final int value = count.getOrDefault(start, 0);
  if (value > 0) {
    for (int i = start; i < start + groupSize; ++i) {
      if (count.merge(i, -value, Integer::sum) < 0)
        return false;
    }
  }
}
```

- `count.merge(i, -value, Integer::sum)` subtracts `value` from the count of card `i`.
- If at any point the count of a card goes below zero, it means we can't form a valid group, so return `false`.

### Final Return

4. **Return `true` if all cards are grouped successfully**:

```java
return true;
```

### Dry Run Example

Let's do a dry run with an example:

- `hand = [1, 2, 3, 6, 2, 3, 4, 7, 8]`
- `groupSize = 3`

#### Step-by-Step Execution

1. **Count frequencies**:
   - After the loop, `count` will be:
     - `{1=1, 2=2, 3=2, 4=1, 6=1, 7=1, 8=1}`

2. **Form groups**:

   - Start with `start = 1`:
     - `value = 1`
     - Form group `[1, 2, 3]`
     - Update `count`:
       - `{1=0, 2=1, 3=1, 4=1, 6=1, 7=1, 8=1}`
   
   - Move to `start = 2`:
     - `value = 1`
     - Form group `[2, 3, 4]`
     - Update `count`:
       - `{1=0, 2=0, 3=0, 4=0, 6=1, 7=1, 8=1}`

   - Move to `start = 3`:
     - `value = 0`
     - Skip

   - Move to `start = 4`:
     - `value = 0`
     - Skip

   - Move to `start = 6`:
     - `value = 1`
     - Form group `[6, 7, 8]`
     - Update `count`:
       - `{1=0, 2=0, 3=0, 4=0, 6=0, 7=0, 8=0}`

3. **All cards are grouped successfully**:
   - Return `true`.

The dry run shows that the method works as expected for the given example.
