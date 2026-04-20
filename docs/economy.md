# Economy System Documentation

## Overview

The Towns economy system determines how item values are calculated for caravans, vendors, and military equipment.

There are three main stages involved in final item pricing:

1. Base item value from the item definition
2. Military stat modifier pricing from `prices.xml`
3. Caravan vendor markup from caravan XML (`pricePCT`)

This means the final displayed shop price is not simply the item value from the item definition.

---

## Core Pricing Flow

### Final Caravan Shop Price

```text
Final Price =
(
    Base Item Value
    + Military Attribute Value
)
× Caravan Price Percentage
```

This happens in two separate systems:

* `PricesManager.java` → calculates true item value
* `CaravanData.java` → applies caravan vendor markup

---

## Step 1: Base Item Value

### Location

```text
data/items/*.xml
```

### Example

```xml
<item>
    <value>300</value>
</item>
```

This is the starting value for all pricing.

Example:

Iron Axe base value = `300`

This value is retrieved through:

```java
ItemManager.getItem(item.getIniHeader()).getValue()
```

---

## Step 2: Military Attribute Pricing

### Location

```text
data/prices.xml
```

### Example

```xml
<attack>18</attack>
<defense>18</defense>
<damage>18</damage>
<LOS>1</LOS>
```

These values define how much stat value is required before +1 is added to item price.

### Formula

```text
Bonus Price =
(attack / attackRule)
+ (defense / defenseRule)
+ (damage / damageRule)
+ (los / losRule)
```

### Important

This uses **integer division**.

### Example

```text
192 / 18 = 10
175 / 18 = 9
292 / 18 = 16
0 / 1 = 0
```

### Total Military Bonus

```text
10 + 9 + 16 + 0 = 35
```

So:

```text
300 + 35 = 335
```

---

## Step 3: Caravan Vendor Markup

### Location

```text
data/caravans/*.xml
```

### Example

```xml
<pricePCT>1d100+300</pricePCT>
```

This means caravan prices are randomly increased by:

```text
301% → 400%
```

The actual value is rolled using:

```java
Utils.launchDice()
```

and stored as:

```java
caravanData.setPricePCT(...)
```

Then applied during menu generation:

```java
(caravanItemPrice * caravanData.getPricePCT()) / 100
```

This happens inside:

```java
createCaravanMenu()
```

---

## Real Example

### Item Stats — Iron Axe

```text
Base value = 300
Attack = 192
Defense = 175
Damage = 292
LOS = 0
```

### `prices.xml`

```xml
<attack>18</attack>
<defense>18</defense>
<damage>18</damage>
<LOS>1</LOS>
```

---

## Military Price Calculation

```text
300
+ (192 / 18)
+ (175 / 18)
+ (292 / 18)
+ (0 / 1)

= 300 + 10 + 9 + 16 + 0

= 335
```

---

## Caravan Example

If caravan roll is:

```text
pricePCT = 316%
```

Final vendor price:

```text
(335 × 316) / 100

= 1058
```

This matches observed in-game pricing.
