# Example
Consider we have three classes:

ProductController.java
ProductModel.java
BlackFridayDiscount.java

``` java
public class ProductController {
  public void saveProduct(ProductDTO product, bool isBlackFriday) {
    DiscountInterface discount;
    
    if(isBlackFriday) discount = new BlackFridayDiscount;
    else discount = new RegularDiscount;
  
    Product product = Product.createFromProduct(productDto);
    product.applyDiscount(discount);
    int id = product.saveToDB();
    
    return product;
  }
}

public class ProductModel {
  public void saveToDB() {    
    DB.save(product);
  }
  
  public function applyDiscount(DiscountInterface discount) {
    this.finalPrice = discount.getDiscount(this);
  }
  
  public static Product createFromProduct(ProductDTO productDTO) {
    Product product = new Product();
    product.price = productDTO.price;
    product.title = productDTO.title;
    product.isHot = productDTO.hotDiscount;
  }
}

public class BlackFridayDiscount implements DiscountInterface {
  public static function getDiscount(Product product) {
    if(product.isHot && product.price >= 200) return product.price * 0.8; // 80% discount
    else if(product.price < 100) return 0;
    else return product.price * 0.5; // 50% discount
  }
}

public class RegularDiscount implements DiscountInterface {
  public static function getDiscount(Product product) {
    if(product.isHot && product.price >= 500) return product.price * 0.5; // 50% discount
    else if(product.price < 100) return 0;
    else return product.price * 0.2; // 20% discount
  }
}
```

## Unit testing
In this example I would unit test `RegularDiscount` and `BlackFridayDiscount`.
I will create some dummy Product objects with different values and check how code works, 
will it return 50% in case our product is not hot and worth more than 100. What if product price is less than 0. 
What if price is exactly 100. And so on. All edge cases.

## Integration testing
I will test in integration level `ProductModel`. 
I will test that I can create the Product, save it to the database, it will be saved to the right table, I can create another one, 
it will apply discount I have passed, and I can delete then the record, and no exception will be thrown.

## Functional testing
I will test in functional level `ProductController` itself. Actually what I will do, 
I will call the /product/save-product endpoint with regular data and black friday date, 
and then call /product/get-product endpoint. And test that it was successfully created and proper price is set up.

# Conclusion
Having all those levels of testing I am confident that `BlackFridayDiscount`, `RegularDiscount` tested for all edge cases. 
The `ProductModel` is properly uses database and discount. 
And `ProductController` is using `ProductModel` correctly, and passes correct discount based on the date.
