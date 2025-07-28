# Test-Driven Development (TDD) Workflow Example

This example demonstrates how to use Claude Code agents in a TDD workflow where one agent writes comprehensive tests while another implements the code to pass those tests.

## The TDD Agent Pattern

This pattern leverages two specialized agents:
1. **Test Writer**: Focuses on creating comprehensive test suites
2. **Implementation Developer**: Writes minimal code to pass tests

## Basic TDD Workflow

### Step 1: Define Requirements

```markdown
I need to implement a shopping cart system with the following features:
- Add items to cart
- Remove items from cart
- Update item quantities
- Calculate total with tax
- Apply discount codes
- Clear cart

Use TDD approach with two agents:
1. Test Writer: Create comprehensive test suite first
2. Implementation Developer: Write code to pass all tests
```

### Step 2: Test Writer Agent Output

```python
# test_shopping_cart.py
import pytest
from decimal import Decimal
from datetime import datetime
from shopping_cart import ShoppingCart, CartItem, DiscountCode

class TestShoppingCart:
    """Comprehensive test suite for ShoppingCart functionality"""
    
    @pytest.fixture
    def empty_cart(self):
        """Provide empty cart for tests"""
        return ShoppingCart()
    
    @pytest.fixture
    def sample_items(self):
        """Sample items for testing"""
        return [
            CartItem("PROD-001", "Laptop", Decimal("999.99"), 1),
            CartItem("PROD-002", "Mouse", Decimal("29.99"), 2),
            CartItem("PROD-003", "Keyboard", Decimal("79.99"), 1)
        ]

    # Test: Adding Items
    def test_add_single_item_to_empty_cart(self, empty_cart):
        """Should add item to empty cart"""
        item = CartItem("PROD-001", "Laptop", Decimal("999.99"), 1)
        empty_cart.add_item(item)
        
        assert len(empty_cart.items) == 1
        assert empty_cart.items[0].product_id == "PROD-001"
        assert empty_cart.items[0].quantity == 1

    def test_add_multiple_different_items(self, empty_cart, sample_items):
        """Should handle multiple different items"""
        for item in sample_items:
            empty_cart.add_item(item)
        
        assert len(empty_cart.items) == 3
        assert empty_cart.get_item_count() == 4  # 1 + 2 + 1

    def test_add_existing_item_updates_quantity(self, empty_cart):
        """Should update quantity when adding existing item"""
        item1 = CartItem("PROD-001", "Laptop", Decimal("999.99"), 1)
        item2 = CartItem("PROD-001", "Laptop", Decimal("999.99"), 2)
        
        empty_cart.add_item(item1)
        empty_cart.add_item(item2)
        
        assert len(empty_cart.items) == 1
        assert empty_cart.items[0].quantity == 3

    def test_add_item_with_invalid_quantity(self, empty_cart):
        """Should reject items with invalid quantity"""
        with pytest.raises(ValueError, match="Quantity must be positive"):
            item = CartItem("PROD-001", "Laptop", Decimal("999.99"), 0)
            empty_cart.add_item(item)

    # Test: Removing Items
    def test_remove_existing_item(self, empty_cart, sample_items):
        """Should remove item from cart"""
        empty_cart.add_item(sample_items[0])
        empty_cart.add_item(sample_items[1])
        
        empty_cart.remove_item("PROD-001")
        
        assert len(empty_cart.items) == 1
        assert empty_cart.items[0].product_id == "PROD-002"

    def test_remove_nonexistent_item(self, empty_cart):
        """Should handle removing item not in cart"""
        with pytest.raises(KeyError, match="Item not found in cart"):
            empty_cart.remove_item("PROD-999")

    # Test: Updating Quantities
    def test_update_item_quantity(self, empty_cart, sample_items):
        """Should update quantity of existing item"""
        empty_cart.add_item(sample_items[0])
        
        empty_cart.update_quantity("PROD-001", 5)
        
        assert empty_cart.items[0].quantity == 5

    def test_update_quantity_to_zero_removes_item(self, empty_cart, sample_items):
        """Should remove item when quantity set to zero"""
        empty_cart.add_item(sample_items[0])
        
        empty_cart.update_quantity("PROD-001", 0)
        
        assert len(empty_cart.items) == 0

    # Test: Total Calculation
    def test_calculate_subtotal(self, empty_cart, sample_items):
        """Should calculate correct subtotal"""
        for item in sample_items:
            empty_cart.add_item(item)
        
        # 999.99 + (29.99 * 2) + 79.99 = 1139.96
        assert empty_cart.get_subtotal() == Decimal("1139.96")

    def test_calculate_total_with_tax(self, empty_cart, sample_items):
        """Should calculate total with tax"""
        for item in sample_items:
            empty_cart.add_item(item)
        
        # Subtotal: 1139.96, Tax (10%): 113.996, Total: 1253.956
        total = empty_cart.get_total(tax_rate=Decimal("0.10"))
        assert total == Decimal("1253.96")  # Rounded to 2 decimals

    def test_empty_cart_total_is_zero(self, empty_cart):
        """Should return zero for empty cart"""
        assert empty_cart.get_subtotal() == Decimal("0")
        assert empty_cart.get_total() == Decimal("0")

    # Test: Discount Codes
    def test_apply_percentage_discount(self, empty_cart, sample_items):
        """Should apply percentage discount correctly"""
        for item in sample_items:
            empty_cart.add_item(item)
        
        discount = DiscountCode("SAVE20", "percentage", Decimal("20"))
        empty_cart.apply_discount(discount)
        
        # Subtotal: 1139.96, Discount: 20%, Total: 911.968
        assert empty_cart.get_total() == Decimal("911.97")

    def test_apply_fixed_discount(self, empty_cart, sample_items):
        """Should apply fixed amount discount"""
        for item in sample_items:
            empty_cart.add_item(item)
        
        discount = DiscountCode("SAVE50", "fixed", Decimal("50"))
        empty_cart.apply_discount(discount)
        
        # Subtotal: 1139.96, Discount: 50, Total: 1089.96
        assert empty_cart.get_total() == Decimal("1089.96")

    def test_discount_cannot_exceed_total(self, empty_cart):
        """Should not allow negative totals with discount"""
        item = CartItem("PROD-001", "Cheap Item", Decimal("10.00"), 1)
        empty_cart.add_item(item)
        
        discount = DiscountCode("SAVE100", "fixed", Decimal("100"))
        empty_cart.apply_discount(discount)
        
        assert empty_cart.get_total() == Decimal("0")

    def test_expired_discount_code(self, empty_cart, sample_items):
        """Should reject expired discount codes"""
        for item in sample_items:
            empty_cart.add_item(item)
        
        expired_date = datetime(2020, 1, 1)
        discount = DiscountCode("OLD20", "percentage", Decimal("20"), expired_date)
        
        with pytest.raises(ValueError, match="Discount code has expired"):
            empty_cart.apply_discount(discount)

    # Test: Clear Cart
    def test_clear_cart(self, empty_cart, sample_items):
        """Should remove all items from cart"""
        for item in sample_items:
            empty_cart.add_item(item)
        
        empty_cart.clear()
        
        assert len(empty_cart.items) == 0
        assert empty_cart.get_subtotal() == Decimal("0")
        assert empty_cart.discount_code is None

    # Test: Edge Cases
    def test_handle_very_large_quantities(self, empty_cart):
        """Should handle large quantities correctly"""
        item = CartItem("PROD-001", "Bulk Item", Decimal("0.01"), 1000000)
        empty_cart.add_item(item)
        
        assert empty_cart.get_subtotal() == Decimal("10000.00")

    def test_precise_decimal_calculations(self, empty_cart):
        """Should maintain precision in calculations"""
        # Testing with prices that could cause floating point errors
        item1 = CartItem("PROD-001", "Item 1", Decimal("0.1"), 3)
        item2 = CartItem("PROD-002", "Item 2", Decimal("0.2"), 3)
        
        empty_cart.add_item(item1)
        empty_cart.add_item(item2)
        
        # 0.1 * 3 + 0.2 * 3 = 0.3 + 0.6 = 0.9 (exact)
        assert empty_cart.get_subtotal() == Decimal("0.9")

    # Test: Concurrent Modifications
    def test_thread_safe_operations(self, empty_cart):
        """Should handle concurrent modifications safely"""
        # This is a placeholder for thread safety testing
        # Real implementation would use threading
        pass

    # Test: Cart State
    def test_cart_is_empty_check(self, empty_cart, sample_items):
        """Should correctly report if cart is empty"""
        assert empty_cart.is_empty() is True
        
        empty_cart.add_item(sample_items[0])
        assert empty_cart.is_empty() is False
        
        empty_cart.clear()
        assert empty_cart.is_empty() is True

    def test_cart_serialization(self, empty_cart, sample_items):
        """Should serialize cart to JSON"""
        for item in sample_items:
            empty_cart.add_item(item)
        
        cart_json = empty_cart.to_json()
        assert "items" in cart_json
        assert len(cart_json["items"]) == 3
        assert cart_json["subtotal"] == "1139.96"

# Run tests with coverage
# pytest test_shopping_cart.py --cov=shopping_cart --cov-report=html
```

### Step 3: Implementation Developer Agent Output

```python
# shopping_cart.py
from decimal import Decimal, ROUND_HALF_UP
from datetime import datetime
from typing import List, Optional, Dict
import json

class CartItem:
    """Represents an item in the shopping cart"""
    
    def __init__(self, product_id: str, name: str, price: Decimal, quantity: int):
        if quantity <= 0:
            raise ValueError("Quantity must be positive")
        
        self.product_id = product_id
        self.name = name
        self.price = price
        self.quantity = quantity
    
    def get_total(self) -> Decimal:
        """Calculate total price for this item"""
        return self.price * self.quantity

class DiscountCode:
    """Represents a discount code"""
    
    def __init__(self, code: str, discount_type: str, value: Decimal, 
                 expires_at: Optional[datetime] = None):
        self.code = code
        self.discount_type = discount_type  # 'percentage' or 'fixed'
        self.value = value
        self.expires_at = expires_at
    
    def is_valid(self) -> bool:
        """Check if discount code is still valid"""
        if self.expires_at is None:
            return True
        return datetime.now() < self.expires_at
    
    def calculate_discount(self, subtotal: Decimal) -> Decimal:
        """Calculate discount amount based on subtotal"""
        if self.discount_type == 'percentage':
            return subtotal * (self.value / 100)
        else:  # fixed
            return min(self.value, subtotal)

class ShoppingCart:
    """Shopping cart implementation with TDD approach"""
    
    def __init__(self):
        self.items: List[CartItem] = []
        self.discount_code: Optional[DiscountCode] = None
    
    def add_item(self, item: CartItem) -> None:
        """Add item to cart or update quantity if exists"""
        existing_item = self._find_item(item.product_id)
        
        if existing_item:
            existing_item.quantity += item.quantity
        else:
            self.items.append(item)
    
    def remove_item(self, product_id: str) -> None:
        """Remove item from cart"""
        item = self._find_item(product_id)
        if not item:
            raise KeyError(f"Item not found in cart: {product_id}")
        
        self.items.remove(item)
    
    def update_quantity(self, product_id: str, quantity: int) -> None:
        """Update item quantity, remove if zero"""
        if quantity < 0:
            raise ValueError("Quantity cannot be negative")
        
        if quantity == 0:
            self.remove_item(product_id)
            return
        
        item = self._find_item(product_id)
        if not item:
            raise KeyError(f"Item not found in cart: {product_id}")
        
        item.quantity = quantity
    
    def get_subtotal(self) -> Decimal:
        """Calculate subtotal before tax and discounts"""
        return sum(item.get_total() for item in self.items)
    
    def get_total(self, tax_rate: Decimal = Decimal("0")) -> Decimal:
        """Calculate total including tax and discounts"""
        subtotal = self.get_subtotal()
        
        # Apply discount if present
        if self.discount_code:
            discount_amount = self.discount_code.calculate_discount(subtotal)
            subtotal -= discount_amount
        
        # Apply tax
        tax_amount = subtotal * tax_rate
        total = subtotal + tax_amount
        
        # Round to 2 decimal places
        return total.quantize(Decimal("0.01"), rounding=ROUND_HALF_UP)
    
    def apply_discount(self, discount_code: DiscountCode) -> None:
        """Apply discount code to cart"""
        if not discount_code.is_valid():
            raise ValueError("Discount code has expired")
        
        self.discount_code = discount_code
    
    def clear(self) -> None:
        """Remove all items and discounts from cart"""
        self.items = []
        self.discount_code = None
    
    def get_item_count(self) -> int:
        """Get total number of items in cart"""
        return sum(item.quantity for item in self.items)
    
    def is_empty(self) -> bool:
        """Check if cart is empty"""
        return len(self.items) == 0
    
    def to_json(self) -> Dict:
        """Serialize cart to JSON-compatible dict"""
        return {
            "items": [
                {
                    "product_id": item.product_id,
                    "name": item.name,
                    "price": str(item.price),
                    "quantity": item.quantity,
                    "total": str(item.get_total())
                }
                for item in self.items
            ],
            "subtotal": str(self.get_subtotal()),
            "discount": {
                "code": self.discount_code.code,
                "type": self.discount_code.discount_type,
                "value": str(self.discount_code.value)
            } if self.discount_code else None,
            "item_count": self.get_item_count()
        }
    
    def _find_item(self, product_id: str) -> Optional[CartItem]:
        """Find item by product ID"""
        for item in self.items:
            if item.product_id == product_id:
                return item
        return None
```

## Advanced TDD Patterns

### 1. API Endpoint TDD

```markdown
"Implement REST API for user management using TDD:

Test Writer Agent:
- Write API tests using supertest/pytest
- Cover all CRUD operations
- Include authentication tests
- Test error scenarios

Implementation Agent:
- Create minimal API to pass tests
- Focus on correctness over optimization"
```

### 2. React Component TDD

```markdown
"Build a SearchableList component using TDD:

Test Writer Agent:
- Use React Testing Library
- Test user interactions
- Test accessibility
- Test edge cases

Implementation Agent:
- Build component incrementally
- Make each test pass before moving on"
```

### 3. Complex Algorithm TDD

```markdown
"Implement a rate limiting algorithm using TDD:

Test Writer Agent:
- Test basic rate limiting
- Test burst handling
- Test distributed scenarios
- Test edge cases

Implementation Agent:
- Start with simple implementation
- Refactor for performance after tests pass"
```

## Benefits of Agent-Based TDD

1. **Separation of Concerns**: Test writer thinks like a user, implementation focuses on code
2. **Comprehensive Coverage**: Test writer isn't biased by implementation details
3. **Better Tests**: Dedicated focus on test quality
4. **Faster Development**: Parallel thinking about tests and implementation
5. **Learning Tool**: See how different agents approach the same problem

## TDD Workflow Variations

### 1. Behavior-Driven Development (BDD)

```gherkin
# Test Writer creates features
Feature: Shopping Cart
  Scenario: Adding items to cart
    Given an empty shopping cart
    When I add a laptop for $999.99
    Then the cart should contain 1 item
    And the total should be $999.99
```

### 2. Property-Based Testing

```python
# Test Writer creates property tests
from hypothesis import given, strategies as st

@given(
    price=st.decimals(min_value=0.01, max_value=10000, places=2),
    quantity=st.integers(min_value=1, max_value=100)
)
def test_cart_total_properties(empty_cart, price, quantity):
    """Total should always equal price * quantity"""
    item = CartItem("PROD-TEST", "Test Item", price, quantity)
    empty_cart.add_item(item)
    
    assert empty_cart.get_subtotal() == price * quantity
```

### 3. Mutation Testing

```markdown
"After implementation:

Mutation Tester Agent:
- Run mutation testing
- Identify gaps in test coverage
- Suggest additional tests

Test Improver Agent:
- Add tests to catch mutations
- Improve test quality"
```

## Integration with CI/CD

```yaml
# .github/workflows/tdd.yml
name: TDD Workflow

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Run Tests First
        run: |
          pytest test_*.py
          
      - name: Check Implementation
        run: |
          pytest test_*.py --cov=. --cov-fail-under=90
          
      - name: Mutation Testing
        run: |
          mutmut run
```

## Tips for Effective TDD with Agents

1. **Clear Requirements**: Give test writer detailed specifications
2. **Iterative Approach**: Work in small cycles
3. **Refactoring Phase**: Let implementation agent refactor after green
4. **Test Quality**: Review tests before implementation
5. **Documentation**: Have test writer document test intentions

Remember: The goal of TDD is not just passing tests, but driving good design through tests. The agent separation helps maintain this discipline by preventing implementation details from influencing test design.