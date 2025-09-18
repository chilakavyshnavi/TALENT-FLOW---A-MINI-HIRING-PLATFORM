# Fix for Empty String Values in Radix UI Select Components

## Problem
Radix UI Select components throw an error when SelectItem components have empty string values (`""`). This is because Radix UI requires non-empty string values for proper identification and selection.

## Solution

### Before (Problematic Code)
```jsx
<Select value={status} onValueChange={setStatus}>
  <SelectTrigger>
    <SelectValue placeholder="Select status" />
  </SelectTrigger>
  <SelectContent>
    <SelectItem value="">All</SelectItem>  {/* This causes the error */}
    <SelectItem value="active">Active</SelectItem>
    <SelectItem value="inactive">Inactive</SelectItem>
  </SelectContent>
</Select>
```

### After (Fixed Code)
```jsx
<Select value={status} onValueChange={setStatus}>
  <SelectTrigger>
    <SelectValue placeholder="Select status" />
  </SelectTrigger>
  <SelectContent>
    <SelectItem value="all">All</SelectItem>  {/* Use "all" instead of "" */}
    <SelectItem value="active">Active</SelectItem>
    <SelectItem value="inactive">Inactive</SelectItem>
  </SelectContent>
</Select>
```

### Alternative Solution with Special Handling
```jsx
const handleValueChange = (value) => {
  // Convert "all" back to empty string for your logic if needed
  const actualValue = value === "all" ? "" : value;
  setStatus(actualValue);
};

const displayValue = status === "" ? "all" : status;

<Select value={displayValue} onValueChange={handleValueChange}>
  <SelectTrigger>
    <SelectValue placeholder="Select status" />
  </SelectTrigger>
  <SelectContent>
    <SelectItem value="all">All</SelectItem>
    <SelectItem value="active">Active</SelectItem>
    <SelectItem value="inactive">Inactive</SelectItem>
  </SelectContent>
</Select>
```

## Key Changes
1. Replace empty string values (`""`) with meaningful non-empty strings (e.g., `"all"`, `"none"`, `"default"`)
2. Update the logic that handles these values to account for the new string values
3. If you need to maintain empty string logic elsewhere, convert between the display value and actual value in the change handler

## Common Patterns to Fix
- `value=""` → `value="all"` or `value="none"`
- `value=""` for default/placeholder options → `value="default"`
- Empty string in filter/search contexts → `value="show-all"`

Apply these changes to all Select components that currently use empty string values.