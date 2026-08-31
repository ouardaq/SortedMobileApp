# Sorted

An Android app that teaches sorting algorithms by tracing them. Pick an algorithm, give it a list of numbers, and it walks you through every comparison and every swap — not just the final sorted array.

## What it does

Five algorithms, each presented three ways:

| Algorithm | |
| --------- | - |
| Bubble sort | Insertion sort |
| Selection sort | Merge sort |
| Quick sort | |

**Theory** — what the algorithm does and how it behaves.

**Code** — the implementation, so the trace can be read against the source that produced it.

**Steps** — the trace. For your own input, every comparison is narrated in order:

```
----- comparison 3 : comparing 8 and 5 -----
(8>5) TRUE , swap positions
  °° swap 2 : [3, 5, 8, 9, 1]
```

This is the part that makes the app worth having. A visualiser shows you bars moving; this shows you the decision at each step and what the array looked like after it, which is what you need when the algorithm is the thing you are trying to learn.

## Structure

```
MainActivity        animated splash, opens the dashboard
Dashboard           algorithm picker
Sorting_activity    hosts the three tabs for the chosen algorithm
  TheoryFragment    explanation
  CodeFragment      source listing
  StepsFragment     the trace, via StepListAdapter
  TriFragment       input entry

BubbleSort / InsertionSort / SelectionSort / MergeSort / QuickSort
                    each exposes steps(Integer[]) returning the narration
TheorySort          theory text
AlgorithmCode       code listings
```

The sorting classes return an `ArrayList<String>` of narration rather than printing or sorting in place, which keeps the algorithms independent of the UI that displays them — the same `steps()` output feeds the RecyclerView.

## Building

**Prerequisites:** Android Studio, JDK 8, and a device or emulator on API 16 or higher.

```bash
git clone https://github.com/ouardaq/SortedMobileApp.git
cd SortedMobileApp/sorted
```

Open the `sorted/` directory in Android Studio, let Gradle sync, and run. Or:

```bash
./gradlew assembleDebug
```

| | |
| --- | --- |
| `compileSdkVersion` | 29 |
| `minSdkVersion` | 16 |
| UI | AppCompat, Material Components, ConstraintLayout, RecyclerView, CardView |

## Notes

This is 2020 coursework and the toolchain shows it — `compileSdkVersion 29` and the `apply plugin:` Gradle syntax both predate current Android Studio defaults, so a modern install will offer to upgrade them on first open.

The swap counter is inconsistent across the five classes, which is worth tidying if this is ever
picked back up:

- `BubbleSort.Nswaps` and `SelectionSort.Nswaps` are `public static int` fields incremented on
  every swap and never reset, so the `°° swap N` numbering in the trace keeps climbing across
  successive sorts within a single session rather than restarting at 1.
- `MergeSort` declares `Nswaps` but never touches it.
- `InsertionSort`'s only reference to it sits inside a commented-out `swap()` method.
- `QuickSort` has no counter at all.
