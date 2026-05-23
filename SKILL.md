---
name: compose-glassmorphism-theme
description: Guidelines, design tokens, and components for "Trendy Glassmorphism 2.0" in Jetpack Compose and Compose Multiplatform. Use when building premium glassmorphic cards, custom 3D scroll pickers, high-performance bezier line charts, or theme-reactive animated ambient backdrops.
---

# 💎 Trendy Glassmorphism 2.0 for Jetpack Compose & Compose Multiplatform

This skill codifies the core guidelines, color tokens, rendering parameters, and custom components required to build premium, high-performance "Glassmorphism 2.0" layouts in Jetpack Compose and Compose Multiplatform (KMP).

## 🎨 Design Guidelines & Color Tokens

Supports high-contrast dynamic blending for both Light Mode and Dark Mode:

*   **Dynamic Theme Backdrops:**
    *   *Dark Mode:* Base background is Midnight black (`#08080C`), surfaces are `#0F0F14`.
    *   *Light Mode:* Base background is Pale Lavender-Grey (`#F8F8FC`), surfaces are `#FFFFFF`.
*   **Animated Ambient Glows (`AnimatedAuraBackground.kt`):**
    *   Draws soft, organic, infinite drifting primary, secondary, and tertiary radial gradients across the entire canvas edge-to-edge (utilizing `drawRect` to prevent circular clipping).
    *   **Mandatory Blending Rules:**
        *   *Dark Mode:* Must use `BlendMode.Plus` to create intense, glowing, luminous overlaps.
        *   *Light Mode:* Must use **`BlendMode.SrcOver`** (standard transparency blending) on top of the pale grey background. *Never* use additive blend modes (`Screen` or `Plus`) in light mode as they wash out to solid white!
*   **Glass Surface Specs:**
    *   *Dark Mode:* `glassSurface = Color(0x1AFFFFFF)` (10% White), `glassBorder = Color(0x26FFFFFF)` (15% White), and `glassShadow = Color(0x14000000)` (8% Black).
    *   *Light Mode:* **`glassSurface = Color(0x3DFFFFFF)` (24% White)** for translucent frosted glass, **`glassBorder = Color(0x14000000)` (8% Black)** for super-thin rim definition against moving pastel gradients, and **`glassShadow = Color(0x0A000000)` (4% Black)** for soft light-mode depth.

## ⚠️ The Transparent Scaffold Mandate
Because the animated aura background runs globally at the root of the app, any standard `Scaffold` container will completely block out the animated gradients with its own solid default background color.
*   **Mandate:** Every screen and the main root `Scaffold` **MUST** use `containerColor = Color.Transparent` so the single root global animated background can flow seamlessly behind all screen layouts:
    ```kotlin
    Scaffold(
        containerColor = Color.Transparent,
        ...
    )
    ```

## 🛠️ Custom Premium Components

### 1. High-Fidelity GlassCard
```kotlin
@Composable
fun GlassCard(
    modifier: Modifier = Modifier,
    border: BorderStroke? = null,
    content: @Composable ColumnScope.() -> Unit,
) {
    val extColors = AuraWellTheme.extendedColors
    val isDark = extColors.glassSurface != Color(0xB3FFFFFF) // 70% white check
    
    Surface(
        modifier = modifier
            .fillMaxWidth()
            .shadow(
                elevation = 12.dp,
                shape = RoundedCornerShape(24.dp),
                ambientColor = extColors.glassShadow,
                spotColor = extColors.glassShadow
            ),
        shape = RoundedCornerShape(24.dp),
        color = extColors.glassSurface,
        border = border ?: BorderStroke(
            width = 0.5.dp,
            brush = Brush.verticalGradient(
                colors = if (isDark) {
                    listOf(Color.White.copy(0.18f), Color.White.copy(0.03f))
                } else {
                    listOf(Color.Black.copy(0.08f), Color.Black.copy(0.02f))
                }
            )
        ),
    ) {
        Column(modifier = Modifier.padding(16.dp), content = content)
    }
}
```

### 2. Tactile 3D Pickers (Horizontal & Vertical Wheel)
Replaces flat sliders and standard text fields with premium horizontal or vertical cylinder-projected scrolling pickers.

*   **Cylinder Projection Math:** Items scale and fade as they move away from the center: apply `rotationX` (for vertical wheel) or `rotationY` (for horizontal ruler) based on the item's distance from the viewport center to simulate a rotating 3D cylinder.
*   **Perfect Centering:** Use `contentPadding` on the `LazyColumn`/`LazyRow` equal to `(viewportWidth - itemWidth) / 2` to ensure the first/last items center perfectly, preventing rounding or scaling math errors.
*   **Initialization Guard:** Use an `isInitialized: Boolean` state flag to suppress selection updates during the initial scroll layout frame, preventing loaded database values from being overwritten with default index `0`.

### 3. Zero-Recomposition Line Charts (Caching Drawing)
*   **Hygiene Rule:** Never allocate `Path()` or calculate grid coordinates directly inside a `Canvas` draw pass. This creates major garbage collection (GC) churn and micro-stutters during lists scrolling.
*   **Mandate:** Wrap custom charts inside a `Spacer` utilizing the **`drawWithCache`** modifier. This pre-allocates and caches all Bezier paths, grid coordinates, and color brushes **exactly once** when dimensions change, keeping frame-level allocations during scrolling at **exactly 0**.
*   **Bezier Rendering:** Draw paths using cubic Bezier curves (`cubicTo`) instead of rigid straight lines for fluid, organic visual rhythms.
```
