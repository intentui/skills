# Forms Rules

Intent UI form components are built on `react-aria-components` and follow specific patterns. Always use them instead of raw HTML form elements.

## Form structure with Fieldset

When grouping form fields, use `Fieldset`, `Legend`, `FieldGroup`, and `Text` — not raw `<div>` with `<h1>` or `<p>`.

```tsx
import { Fieldset, Legend, FieldGroup, Label } from "@/components/ui/field"
import { TextField } from "@/components/ui/text-field"
import { Input } from "@/components/ui/input"
import { Text } from "@/components/ui/text"
import { Button } from "@/components/ui/button"

// ✅ Correct — use Fieldset, Legend, and Text
<form>
  <Fieldset>
    <Legend>Login</Legend>
    <Text>
      Login to your account to access your dashboard and manage your settings.
    </Text>

    <TextField name="email">
      <Label>Email</Label>
      <Input type="email" />
    </TextField>
    <TextField name="name">
      <Label>Name</Label>
      <Input />
    </TextField>
  </Fieldset>
  <Button className="mt-6" type="submit">
    Login
  </Button>
</form>

// ❌ Wrong — using raw div, h1, and p
<div className="space-y-6">
  <h1 className="text-xl font-semibold">Login</h1>
  <p className="text-muted-fg text-sm/6">
    Login to your account to access your dashboard and manage your settings.
  </p>
  <form>
    <div className="space-y-6">
      <TextField name="email">
        <Label>Email</Label>
        <Input type="email" />
      </TextField>
      <TextField name="name">
        <Label>Name</Label>
        <Input />
      </TextField>
      <Button type="submit">Login</Button>
    </div>
  </form>
</div>
```

### data-slot="control" for custom rows inside Fieldset

Fieldset uses `[&>*+[data-slot=control]]:mt-6` for spacing. TextField and other form components already have `data-slot="control"`, but if you place a custom `<div>` inside a Fieldset (e.g. for a checkbox + link row), add `data-slot="control"` so it gets the correct spacing.

```tsx
// ✅ Correct — full login form with Fieldset
<Fieldset>
  <Legend>Log in</Legend>
  <Text>Welcome back! Enter your credentials to access your account.</Text>

  <TextField isRequired name="email" type="email">
    <Label>Email</Label>
    <Input placeholder="you@example.com" />
    <FieldError />
  </TextField>

  <TextField isRequired name="password" type="password">
    <Label>Password</Label>
    <Input placeholder="Enter your password" />
    <FieldError />
  </TextField>

  <div data-slot="control" className="flex items-center justify-between">
    <CheckboxField name="remember">
      <Checkbox>Remember me</Checkbox>
    </CheckboxField>
    <Link href="/forgot-password" className="text-sm">
      Forgot password?
    </Link>
  </div>
</Fieldset>

// ❌ Wrong — div without data-slot="control" won't get Fieldset spacing
<div className="flex items-center justify-between">
  <CheckboxField name="remember">
    <Checkbox>Remember me</Checkbox>
  </CheckboxField>
  <Link href="/forgot-password" className="text-sm">
    Forgot password?
  </Link>
</div>
```

### Why?

- **`<Fieldset>`** groups related fields and handles spacing automatically (`[&>*+[data-slot=control]]:mt-6`)
- **`<Legend>`** is the correct heading for a fieldset — not `<h1>` or `<Heading>`
- **`<Text>`** renders with `data-slot="text"` so Fieldset applies `*:data-[slot=text]:mt-1` spacing automatically
- **`data-slot="control"`** on custom divs inside Fieldset ensures they receive the same `mt-6` spacing as form fields
- **`<FieldGroup>`** wraps multiple fields with `space-y-6` — use it when you need to group fields inside a Fieldset
- No need to manually add `space-y-6` to wrapper divs — Fieldset and FieldGroup handle spacing

### Available field components from `@/components/ui/field`

- `Label` — field label with required indicator support
- `Description` — helper text below a field
- `FieldError` — validation error message
- `Fieldset` — groups related form fields
- `FieldGroup` — wraps multiple fields with spacing (`space-y-6`)
- `Legend` — heading for a Fieldset

## Forbidden input types

Never use `type="number"` or `type="date"` on `<Input />`. These HTML input types have inconsistent browser behavior and poor accessibility. Use the dedicated Intent UI components instead:

- **`type="number"`** → Use `NumberField` with `NumberInput` from `@/components/ui/number-field`
- **`type="date"`** → Use `DatePicker` from `@/components/ui/date-picker`

```tsx
// ❌ NEVER do this
<Input type="number" />
<Input type="date" />

// ✅ Use NumberField for numeric values
<NumberField name="quantity">
  <Label>Quantity</Label>
  <NumberInput />
</NumberField>

// ✅ Use DatePicker for dates
<DatePicker name="startDate">
  <Label>Start date</Label>
  <DatePickerTrigger />
</DatePicker>
```

## Text input

Use `TextField` with `Input`, `Label`, and `Description`:

```tsx
import { TextField } from "@/components/ui/text-field"
import { Input } from "@/components/ui/input"
import { Label, Description } from "@/components/ui/field"

// ✅ Basic text field
<TextField>
  <Label>Name</Label>
  <Input placeholder="Enter your name" />
</TextField>

// ✅ With description
<TextField>
  <Label>Email</Label>
  <Input type="email" />
  <Description>We'll never share your email.</Description>
</TextField>

// ✅ Required — always include FieldError when isRequired is set
<TextField isRequired name="email">
  <Label>Email</Label>
  <Input type="email" />
  <FieldError />
</TextField>

// ✅ With description — use Description when you need helper text below a field
<TextField name="email">
  <Label>Email</Label>
  <Input type="email" />
  <Description>Example description for the email field.</Description>
</TextField>

// ✅ Required + description — both can be used together
<TextField isRequired name="email">
  <Label>Email</Label>
  <Input type="email" />
  <FieldError />
  <Description>Example description for the email field.</Description>
</TextField>

// ❌ Wrong — isRequired without FieldError
<TextField isRequired>
  <Label>Username</Label>
  <Input />
</TextField>

// ❌ Wrong — never use raw input
<label>Name</label>
<input type="text" placeholder="Enter your name" />
```

### TextField scope

Use `TextField` for text-like values only, such as `text`, `email`, `password`, `search`, `tel`, and `url`.

- Do not use `TextField` for numeric values like price, amount, quantity, or percentage. Use `NumberField` instead.
- Do not use `TextField` with `<Input type="date" />`. Use `DatePicker` instead.

### isRequired rule

When `isRequired` is set on a field component, you MUST include `<FieldError />` as a child. This ensures validation errors are displayed to the user.

### Description

Use `<Description>` when you need helper text below a field. It can be used on its own or combined with `<FieldError />` when the field is also required.

## Select

Use `Select`, `SelectTrigger`, `SelectContent`, `SelectItem`:

```tsx
import { Select, SelectTrigger, SelectContent, SelectItem } from "@/components/ui/select"

// ✅ Correct
<Select>
  <Label>Country</Label>
  <SelectTrigger />
  <SelectContent>
    <SelectItem id="us">United States</SelectItem>
    <SelectItem id="uk">United Kingdom</SelectItem>
  </SelectContent>
</Select>

// ✅ Controlled — use value/onChange (not selectedKey/onSelectionChange)
<Select value={country} onChange={setCountry}>
  <Label>Country</Label>
  <SelectTrigger />
  <SelectContent>
    <SelectItem id="us">United States</SelectItem>
    <SelectItem id="uk">United Kingdom</SelectItem>
  </SelectContent>
</Select>

// ✅ Uncontrolled — use defaultValue (never defaultSelectedKey)
<Select defaultValue="us">
  <Label>Country</Label>
  <SelectTrigger />
  <SelectContent>
    <SelectItem id="us">United States</SelectItem>
    <SelectItem id="uk">United Kingdom</SelectItem>
  </SelectContent>
</Select>

// ❌ Wrong — selectedKey/onSelectionChange are deprecated
<Select selectedKey={country} onSelectionChange={setCountry}>
  ...
</Select>

// ❌ Wrong — never use defaultSelectedKey
<Select defaultSelectedKey="us">
  ...
</Select>

// ❌ Wrong — never use raw select
<select>
  <option value="us">United States</option>
</select>
```

## ComboBox

```tsx
import { ComboBox, ComboBoxInput, ComboBoxContent, ComboBoxItem } from "@/components/ui/combo-box"

// ✅ Correct
<ComboBox>
  <Label>Framework</Label>
  <ComboBoxInput />
  <ComboBoxContent>
    <ComboBoxItem id="react">React</ComboBoxItem>
    <ComboBoxItem id="vue">Vue</ComboBoxItem>
  </ComboBoxContent>
</ComboBox>

// ✅ Controlled — use value/onChange (not selectedKey/onSelectionChange)
<ComboBox value={framework} onChange={setFramework}>
  <Label>Framework</Label>
  <ComboBoxInput />
  <ComboBoxContent>
    <ComboBoxItem id="react">React</ComboBoxItem>
    <ComboBoxItem id="vue">Vue</ComboBoxItem>
  </ComboBoxContent>
</ComboBox>

// ✅ Uncontrolled — use defaultValue (never defaultSelectedKey)
<ComboBox defaultValue="react">
  <Label>Framework</Label>
  <ComboBoxInput />
  <ComboBoxContent>
    <ComboBoxItem id="react">React</ComboBoxItem>
    <ComboBoxItem id="vue">Vue</ComboBoxItem>
  </ComboBoxContent>
</ComboBox>

// ❌ Wrong — selectedKey/onSelectionChange are deprecated
<ComboBox selectedKey={framework} onSelectionChange={setFramework}>
  ...
</ComboBox>

// ❌ Wrong — never use defaultSelectedKey
<ComboBox defaultSelectedKey="react">
  ...
</ComboBox>
```

## Deprecated props for Select and ComboBox

`selectedKey`, `onSelectionChange`, and `defaultSelectedKey` are **deprecated**. Always use `value`/`onChange` for controlled state and `defaultValue` for uncontrolled state instead.

| Deprecated | Use instead |
|---|---|
| `selectedKey` | `value` |
| `onSelectionChange` | `onChange` |
| `defaultSelectedKey` | `defaultValue` |

## Checkbox, Radio, Switch — the control only holds the label

This is the single most important rule for these three components: **`Checkbox`, `Radio`, and `Switch` render only the label text.** Every prop — `name`, `value`, `isSelected`, `defaultSelected`, `onChange`, `isDisabled`, `isRequired`, `isInvalid`, `isReadOnly`, `isIndeterminate` — goes on the wrapper `CheckboxField` / `RadioField` / `SwitchField`. This applies even to a **single** control, not just groups.

```tsx
// ❌ Wrong — props on the control itself
<Checkbox name="remember">Remember me</Checkbox>
<Radio value="free">Free</Radio>
<Switch isSelected={on} name="dark">Dark mode</Switch>

// ✅ Right — props on the *Field wrapper, label inside the control
<CheckboxField name="remember"><Checkbox>Remember me</Checkbox></CheckboxField>
<RadioField value="free"><Radio>Free</Radio></RadioField>
<SwitchField isSelected={on} name="dark"><Switch>Dark mode</Switch></SwitchField>
```

## Checkbox

```tsx
import { Checkbox, CheckboxField, CheckboxGroup } from "@/components/ui/checkbox"
import { Label } from "@/components/ui/field"

// ✅ Single checkbox — wrap in CheckboxField (name on the field)
<CheckboxField name="terms">
  <Checkbox>Accept terms</Checkbox>
</CheckboxField>

// ✅ Checkbox group — value on each CheckboxField, label inside Checkbox
<CheckboxGroup defaultValue={["music"]} name="interests">
  <Label>Interests</Label>
  <CheckboxField value="music">
    <Checkbox>Music</Checkbox>
  </CheckboxField>
  <CheckboxField value="sports">
    <Checkbox>Sports</Checkbox>
  </CheckboxField>
</CheckboxGroup>

// ❌ Wrong — value/name on <Checkbox>, no CheckboxField wrapper
<CheckboxGroup>
  <Label>Interests</Label>
  <Checkbox value="music">Music</Checkbox>
</CheckboxGroup>
```

## Radio

Each option is wrapped in a `RadioField` and the **`value` goes on the `RadioField`**, not the `Radio`. The `Radio` just holds the label.

```tsx
import { Radio, RadioField, RadioGroup } from "@/components/ui/radio"
import { Label } from "@/components/ui/field"

// ✅ Correct — value on RadioField, label inside Radio
<RadioGroup defaultValue="free">
  <Label>Plan</Label>
  <RadioField value="free">
    <Radio>Free</Radio>
  </RadioField>
  <RadioField value="pro">
    <Radio>Pro</Radio>
  </RadioField>
</RadioGroup>

// ❌ Wrong — value does not go on <Radio>, and there's no RadioField
<RadioGroup>
  <Label>Plan</Label>
  <Radio value="free">Free</Radio>
</RadioGroup>
```

## Textarea

```tsx
import { Textarea } from "@/components/ui/textarea"

// ✅ Correct
<TextField>
  <Label>Message</Label>
  <Textarea />
</TextField>

// ❌ Wrong
<textarea placeholder="Message" />
```

## Number field

```tsx
import { NumberField } from "@/components/ui/number-field"
import { FieldError, Label } from "@/components/ui/field"
import { NumberInput } from "@/components/ui/number-field"

// ✅ Correct
<NumberField name="price">
  <Label>Price</Label>
  <NumberInput />
</NumberField>

// ✅ Required
<NumberField isRequired name="price">
  <Label>Price</Label>
  <NumberInput />
  <FieldError />
</NumberField>

// ❌ Wrong — do not use TextField/Input type="number" for numeric values
<TextField name="price">
  <Label>Price</Label>
  <Input type="number" />
</TextField>
```

## Search field

Two valid inputs inside `SearchField`:

- **`SearchInput`** (from `@/components/ui/search-field`) — includes a magnifier icon and a clear (✕) button. Use this for the standard search look.
- **`Input`** (from `@/components/ui/input`) — a plain input with no icon. Use this when you don't want the magnifier/clear affordances.

```tsx
import { SearchField, SearchInput } from "@/components/ui/search-field"
import { Input } from "@/components/ui/input"
import { Label } from "@/components/ui/field"

// ✅ Standard search field with icon + clear button
<SearchField>
  <Label>Search</Label>
  <SearchInput />
</SearchField>

// ✅ Plain search field (no icon) — use Input
<SearchField aria-label="Search">
  <Input />
</SearchField>

// ✅ No visible label — provide aria-label
<SearchField aria-label="Search" value={value} onChange={setValue}>
  <SearchInput />
</SearchField>
```

## Switch

Wrap `Switch` in a `SwitchField` for form integration (name, value, controlled state, description). Put the label text inside `Switch`.

```tsx
import { Switch, SwitchField } from "@/components/ui/switch"

// ✅ Correct
<SwitchField name="notifications">
  <Switch>Enable notifications</Switch>
</SwitchField>

// ✅ Controlled
<SwitchField isSelected={enabled} onChange={setEnabled} name="notifications">
  <Switch>Enable notifications</Switch>
</SwitchField>
```

## Date & Time

Each date/time component pairs with its own input/trigger subcomponent:

- `DatePicker` → `DatePickerTrigger`
- `DateRangePicker` → `DateRangePickerTrigger`
- `DateField` / `TimeField` → `DateInput` (from `@/components/ui/date-field`)

```tsx
import { DatePicker, DatePickerTrigger } from "@/components/ui/date-picker"
import { DateRangePicker, DateRangePickerTrigger } from "@/components/ui/date-range-picker"
import { TimeField } from "@/components/ui/time-field"
import { DateInput } from "@/components/ui/date-field"
import { FieldError, Label } from "@/components/ui/field"

// ✅ DatePicker
<DatePicker name="startDate">
  <Label>Start date</Label>
  <DatePickerTrigger />
</DatePicker>

// ✅ DatePicker — required (include FieldError)
<DatePicker isRequired name="startDate">
  <Label>Start date</Label>
  <DatePickerTrigger />
  <FieldError />
</DatePicker>

// ✅ DateRangePicker
<DateRangePicker name="range">
  <Label>Dates</Label>
  <DateRangePickerTrigger />
</DateRangePicker>

// ✅ TimeField — uses DateInput
<TimeField name="time">
  <Label>Time</Label>
  <DateInput />
</TimeField>

// ❌ Wrong — do not use TextField/Input type="date" for dates
<TextField name="startDate">
  <Label>Start date</Label>
  <Input type="date" />
</TextField>
```

## Accessibility: aria-label when no visible Label

Every form field, `GridList`, and `Table` **must** have a label. If there is no visible `<Label>`, you **must** provide an `aria-label` prop.

```tsx
// ✅ Visible label — no aria-label needed
<TextField>
  <Label>Email</Label>
  <Input type="email" />
</TextField>

// ✅ No visible label — aria-label required
<TextField aria-label="Email">
  <Input type="email" />
</TextField>

// ✅ SearchField without visible label
<SearchField aria-label="Search products">
  <Input />
</SearchField>

// ✅ NumberField without visible label
<NumberField aria-label="Quantity">
  <NumberInput />
</NumberField>

// ✅ Select without visible label
<Select aria-label="Sort by">
  <SelectTrigger />
  <SelectContent>
    <SelectItem id="newest">Newest</SelectItem>
    <SelectItem id="oldest">Oldest</SelectItem>
  </SelectContent>
</Select>

// ✅ GridList must have aria-label
<GridList aria-label="Shopping cart items">
  <GridListItem>Item 1</GridListItem>
  <GridListItem>Item 2</GridListItem>
</GridList>

// ✅ Table must have aria-label
<Table aria-label="Users">
  <TableHeader>
    <TableColumn>Name</TableColumn>
  </TableHeader>
  <TableBody>
    <TableRow>
      <TableCell>John</TableCell>
    </TableRow>
  </TableBody>
</Table>

// ❌ Wrong — no label and no aria-label
<TextField>
  <Input type="email" />
</TextField>

// ❌ Wrong — GridList without aria-label
<GridList>
  <GridListItem>Item 1</GridListItem>
</GridList>

// ❌ Wrong — Table without aria-label
<Table>
  <TableHeader>
    <TableColumn>Name</TableColumn>
  </TableHeader>
</Table>
```

This rule applies to **all** form components: `TextField`, `NumberField`, `SearchField`, `Select`, `DatePicker`, `DateRangePicker`, `TimeField`, `CheckboxGroup`, `RadioGroup`, `ComboBox`, `TagField`, as well as `GridList` and `Table`.

## Key patterns

1. **Fieldset + Legend** for grouping related fields — not `<div>` + `<h1>`
2. **Text** for form descriptions — not `<p>`
3. **Label and Description** come from `@/components/ui/field` — they work with all form components
4. **Use the correct field primitive**: `TextField` for text-like values, `NumberField` for numeric values, `DatePicker` for calendar dates
5. **Controlled vs uncontrolled**: Use `value`/`onChange` for controlled, `defaultValue` for uncontrolled
6. **Validation**: Use `isRequired`, `isInvalid`, `validate` props — not custom validation wrappers
7. **Disabled/readonly**: Use `isDisabled`, `isReadOnly` props (camelCase, not HTML attributes)
8. **"use client"** directive: Required when using form components with state/hooks
