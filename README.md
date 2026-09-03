# Smart Emergency Resource Management and Analysis System (C)

A menu-driven C application that helps hospital administrators manage emergency resources — medicines, equipment, beds, and supplies — across multiple departments during high-demand situations.

**Course:** CSA0201 – C Programming
**Team:** C. Rushitha (192320084) · D. Thanvika (192320087) · T. Vijay Kumar (192320095) · K. Vinay (192312080) · A. Ajay (192320038)

---

## Overview

Emergency departments need a fast, reliable way to know what's available, what's running low, and where shortages are most severe. This system lets an administrator add, update, search, sort, merge, analyse, and permanently store resource records. Each resource is automatically classified as **Adequate**, **Low**, or **Critical** based on a quantity-vs-threshold rule, and the program generates a consolidated report with total availability, department-wise totals, and items needing replenishment.

**SDG relevance:** SDG 3 (Good Health & Well-being), SDG 9 (Industry, Innovation & Infrastructure), SDG 11 (Sustainable Cities & Communities).

---

## Data Structure

```c
typedef struct {
    int  id;
    char name[50];
    char category[30];     // Medicine, Equipment, Bed, Supply
    char department[30];   // Emergency, ICU, General, Trauma, etc.
    int  quantity;
    int  threshold;
    int  priority;         // 1 = High, 2 = Medium, 3 = Low
} Resource;
```

Stored in a global array `Resource resources[MAX]` (`MAX = 100`), passed to functions by pointer/array reference.

---

## Menu Options

| # | Option | # | Option |
|---|---|---|---|
| 1 | Add New Resource | 8 | Analyse Resource Availability |
| 2 | Update Resource Details | 9 | Display Critical / Low-Stock Resources |
| 3 | Display All Resources | 10 | Generate Consolidated Report |
| 4 | Search (ID / Name / Category) | 11 | Save Records to File |
| 5 | Sort (Quantity / Priority / Department) | 12 | Retrieve Records from File |
| 6 | Merge Two Departments | 13 | Exit |
| 7 | Identify Duplicate Resources | | |

---

## Core Algorithms

- **Search:** linear scan by ID/name/category; recursive binary search by ID on a sorted copy
- **Sort:** bubble sort by quantity, selection sort by priority, bubble sort by department (string compare)
- **Merge:** combines two departments' records, using a **recursive** `isDuplicate()` check to skip IDs already present
- **Duplicate detection:** `findDuplicatesRecursive()` walks the array recursively, comparing each record against the rest
- **Analysis:** `diff = quantity − threshold` → `diff > 5` Adequate · `0 ≤ diff ≤ 5` Low · `diff < 0` Critical
- **Report:** `totalQuantityRecursive()` and `departmentWiseTotalRecursive()` recursively total quantities, written to `report.txt`
- **Persistence:** `saveToFile()` / `loadFromFile()` read/write `resources.txt` as pipe-delimited records

## Concepts Demonstrated

Operators & expressions · if-else/switch · for/while/do-while loops · arrays of structures · string processing (`strcmp`, `strcpy`) · linear & recursive binary search · bubble/selection sort · recursive duplicate elimination · modular functions · recursion · pointers (arithmetic, pass-by-pointer, swap) · file handling · menu-driven `do-while` + `switch-case`

---

## Testing

23 test cases were run covering **normal**, **boundary**, and **invalid-input** scenarios, including:

- Duplicate ID rejected on add (`"Error: a resource with this ID already exists!"`)
- Boundary threshold diff = 2 classified correctly as LOW
- Out-of-range menu choice (`99`) and non-numeric input (`"abc"`) both handled without crashing
- Merge on non-existent departments returns `"No resources found for the given departments."`
- No-duplicate case correctly terminates the recursive check with `"No duplicate resources found."`

Full transcript and screenshots are in `sample_output.txt` and `screenshots/`.

---

## How to Run

**Local GCC**
```bash
git clone <repository-url>
cd smart-emergency-resource-management-c
gcc -Wall -o resmgmt resource_management.c
./resmgmt
```

**Online compiler:** paste `resource_management.c` into [Programiz](https://www.programiz.com/c-programming/online-compiler/) and run.

**Google Colab:**
