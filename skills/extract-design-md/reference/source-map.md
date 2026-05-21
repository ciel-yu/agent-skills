# Source Map

## Web

Check:

- `DESIGN.md` if present
- `tailwind.config.*`
- global CSS files
- CSS custom properties
- theme provider files
- component variant utilities
- Storybook stories
- screenshots or Playwright-rendered pages

Map:

- CSS variables -> DESIGN.md tokens
- Tailwind theme values -> DESIGN.md tokens
- component variants -> `components` entries
- pseudo states -> state component entries, such as `button-primary-hover`

## WPF

Check:

- `App.xaml`
- merged `ResourceDictionary` files
- `Themes/Generic.xaml`
- control styles
- `ControlTemplate` definitions
- `VisualStateManager` states
- `StaticResource` and `DynamicResource` keys

Map:

- `Color` / `SolidColorBrush` -> `colors`
- `FontFamily`, `FontSize`, `FontWeight` -> `typography`
- `Thickness`, `Margin`, `Padding` -> `spacing` or component `padding`
- `CornerRadius` -> `rounded`
- `Style` / `ControlTemplate` -> `components`
- `VisualStateManager` states -> state component entries

## Avalonia

Check:

- `App.axaml`
- `Styles.axaml`
- theme dictionaries
- control themes
- selectors and pseudo-classes
- resource keys

Map:

- `SolidColorBrush` / color resources -> `colors`
- text resources and styles -> `typography`
- `Thickness`, `Margin`, `Padding` -> `spacing` or component `padding`
- `CornerRadius` -> `rounded`
- `ControlTheme` / styles -> `components`
- `:pointerover`, `:pressed`, `:disabled`, `:focus` -> state component entries

## Native or custom UI

Check platform theme resources first. If values are only hard-coded in components, extract repeated values and cite representative files.
