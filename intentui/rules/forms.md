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

### Why?

- **`<Fieldset>`** groups related fields and handles spacing automatically (`[&>*+[data-slot=control]]:mt-6`)
- **`<Legend>`** is the correct heading for a fieldset — not `<h1>` or `<Heading>`
- **`<Text>`** renders with `data-slot="text"` so Fieldset applies `*:data-[slot=text]:mt-1` spacing automatically
- **`<FieldGroup>`** wraps multiple fields with `space-y-6` — use it when you need to group fields inside a Fieldset
- No need to manually add `space-y-6` to wrapper divs — Fieldset and FieldGroup handle spacing

### Available field components from `@/components/ui/field`

- `Label` — field label with required indicator support
- `Description` — helper text below a field
- `FieldError` — validation error message
- `Fieldset` — groups related form fields
- `FieldGroup` — wraps multiple fields with spacing (`space-y-6`)
- `Legend` — heading for a Fieldset

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

// ❌ Wrong — never use raw select
<select>
  <option value="us">United States</option>
</select>
```

## Checkbox

```tsx
import { Checkbox, CheckboxGroup } from "@/components/ui/checkbox"

// ✅ Single checkbox
<Checkbox>Accept terms</Checkbox>

// ✅ Checkbox group
<CheckboxGroup>
  <Label>Interests</Label>
  <Checkbox value="music">Music</Checkbox>
  <Checkbox value="sports">Sports</Checkbox>
</CheckboxGroup>
```

## Radio

```tsx
import { Radio, RadioGroup } from "@/components/ui/radio"

// ✅ Correct
<RadioGroup>
  <Label>Plan</Label>
  <Radio value="free">Free</Radio>
  <Radio value="pro">Pro</Radio>
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

// ✅ Correct
<NumberField>
  <Label>Quantity</Label>
  <Input />
</NumberField>
```

## Search field

```tsx
import { SearchField } from "@/components/ui/search-field"

// ✅ Correct
<SearchField>
  <Label>Search</Label>
  <Input />
</SearchField>
```

## Switch

```tsx
import { Switch } from "@/components/ui/switch"

// ✅ Correct
<Switch>Enable notifications</Switch>
```

## Date & Time

```tsx
import { DatePicker } from "@/components/ui/date-picker"
import { DateRangePicker } from "@/components/ui/date-range-picker"
import { TimeField } from "@/components/ui/time-field"

// ✅ Correct
<DatePicker>
  <Label>Start date</Label>
</DatePicker>
```

## Key patterns

1. **Fieldset + Legend** for grouping related fields — not `<div>` + `<h1>`
2. **Text** for form descriptions — not `<p>`
3. **Label and Description** come from `@/components/ui/field` — they work with all form components
4. **Controlled vs uncontrolled**: Use `value`/`onChange` for controlled, `defaultValue` for uncontrolled
5. **Validation**: Use `isRequired`, `isInvalid`, `validate` props — not custom validation wrappers
6. **Disabled/readonly**: Use `isDisabled`, `isReadOnly` props (camelCase, not HTML attributes)
7. **"use client"** directive: Required when using form components with state/hooks
