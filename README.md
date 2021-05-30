from django.db import models
from django.contrib.auth import get_user_model

User = get_user_model()

# Create your models here.

# Magazine
# Category
# Product
# CartProduct
# Order
# Customer
# Money

class Magazine(models.Model):

    name = models.CharField(max_length=255, verbose_name="Имя магазина")
    slug = models.SlugField(unique=True)

    def __str__(self):
        return self.name


class Category(models.Model):

    name = models.CharField(max_length=255, verbose_name="Категория")
    slug = models.SlugField(unique=True)

    def __str__(self):
        return self.name


class TypeProduct(models.Model):

    type_product = models.CharField(max_length=255, verbose_name="Тип продукта")
    slug = models.SlugField(unique=True)

    def __str__(self):
        return self.type_product

class Product(models.Model):

    magazine = models.ForeignKey(Magazine, on_delete=models.CASCADE)
    category = models.ForeignKey(Category, on_delete=models.CASCADE)
    type_product = models.ForeignKey(TypeProduct, on_delete=models.CASCADE, verbose_name="Тип продукта")
    title = models.CharField(max_length=255, verbose_name="Наименование товара")
    slug = models.SlugField(unique=True)
    image = models.ImageField(verbose_name="Изображение")
    description = models.TextField(verbose_name="Описание", null=True)
    Price = models.DecimalField(max_digits=9, decimal_places=2, null=True, verbose_name="Цена")

    def __str__(self):
        return self.title

class CartProduct(models.Model):

    user = models.ForeignKey("Customer", on_delete=models.CASCADE, verbose_name="Покупатель")
    cart = models.ForeignKey("Cart", on_delete=models.CASCADE, verbose_name="Корзина", related_name="related_cart")
    product = models.ForeignKey(Product, on_delete=models.CASCADE, verbose_name="Товар")
    quantity = models.PositiveIntegerField(verbose_name="Количество", default=1)
    final_price = models.DecimalField(max_digits=9, decimal_places=2, null=True, verbose_name="Общая цена")

class Cart(models.Model):

    owner = models.ForeignKey("Customer", on_delete=models.CASCADE, verbose_name="Владелец")
    products = models.ManyToManyField("CartProduct", blank=True, related_name="related_product")
    total_products = models.PositiveIntegerField(verbose_name="Количество товаров", default=0)
    final_price = models.DecimalField(max_digits=9, decimal_places=2, null=True, verbose_name="Общая цена")

class Customer(models.Model):

    user = models.ForeignKey(User, on_delete=models.CASCADE, verbose_name="Пользователь")
    phone = models.CharField(max_length=255, verbose_name="Номер телефона")
    user_money = models.DecimalField(max_digits=9, decimal_places=2, null=True, verbose_name="Кошелек пользователя")



