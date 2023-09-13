
import 'package:flutter/material.dart';

class ProductDetailsPage extends StatefulWidget {
  final Map<String, Object> product;
  const ProductDetailsPage({super.key, required this.product});

  @override
  State<ProductDetailsPage> createState() => _ProductDetailsPageState();
}

class _ProductDetailsPageState extends State<ProductDetailsPage> {
  int selectedSize = 0;
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Details'),
      ),
      body: Column(
        children: [
          Text(
            widget.product['title'] as String,
            style: Theme.of(context).textTheme.titleLarge,
          ),
          const Spacer(),
          Padding(
            padding: const EdgeInsets.all(8.0),
            child: Image.asset(widget.product['imageUrl'] as String),
          ),
          const Spacer(
            flex: 2,
          ),
          Container(
            height: 250,
            decoration: BoxDecoration(
                color: const Color.fromRGBO(245, 247, 249, 1),
                borderRadius: BorderRadius.circular(40)),
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                Text(
                  '\$${widget.product['price']}',
                  style: Theme.of(context).textTheme.titleLarge,
                ),
                const SizedBox(
                  height: 5,
                ),
                SizedBox(
                  height: 10,
                  child: ListView.builder(
                    scrollDirection: Axis.horizontal,
                    itemCount: (widget.product['sizes'] as List<int>).length,
                    itemBuilder: (context, index) {
                      final size =
                          (widget.product['sizes'] as List<int>)[index];

                      return Padding(
                        padding: const EdgeInsets.all(8.0),
                        child: Chip(
                            label: Text(size.toString()),
                            backgroundColor: selectedSize == size
                                ? Theme.of(context).colorScheme.primary
                                : null),
                      );
                    },
                  ),
                ),
                Padding(
                  padding: const EdgeInsets.all(20.0),
                  child: ElevatedButton(
                      style: ElevatedButton.styleFrom(
                        backgroundColor: Theme.of(context).colorScheme.primary,
                        minimumSize: const Size(double.infinity, 50),
                      ),
                      onPressed: () {},
                      child: const Text(
                        'Add To Cart',
                        style: TextStyle(
                          color: Colors.black,
                          fontSize: 18,
                        ),
                      )),
                ),
              ],
            ),
          )
        ],
      ),
    );
  }
}
import 'package:flutter/material.dart';
import 'package:trade_app/cart_page.dart';
import 'package:trade_app/product_list.dart';

class HomePage extends StatefulWidget {
  const HomePage({super.key});

  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  int currentPage = 1;
  List<Widget> pages = const [
    ProductList(),
    CartPage(),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: IndexedStack(
        index: currentPage,
        children: pages,
      ),
      bottomNavigationBar: BottomNavigationBar(
          iconSize: 35,
          selectedFontSize: 0,
          unselectedFontSize: 0,
          onTap: (value) {
            setState(() {
              currentPage = 0;
            });
          },
          currentIndex: currentPage,
          items: const [
            BottomNavigationBarItem(
              icon: Icon(Icons.home),
              label: '',
            ),
            BottomNavigationBarItem(
              icon: Icon(Icons.shopping_cart),
              label: '',
            )
          ]),
    );
  }
}
import 'package:flutter/material.dart';
import 'package:trade_app/global_variables.dart';

class CartPage extends StatelessWidget {
  const CartPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: const Text('Cart'),
        ),
        body: ListView.builder(
            itemCount: cart.length,
            itemBuilder: (context, index) {
              final cartItem = cart[index];

              return ListTile(
                leading: CircleAvatar(
                  backgroundImage: AssetImage(cartItem['imageUrl'] as String),
                  radius: 30,
                ),
                trailing: IconButton(
                    onPressed: () {},
                    icon: const Icon(
                      Icons.delete,
                      color: Colors.red,
                    )),
                title: Text(
                  cartItem['title'].toString(),
                  style: Theme.of(context).textTheme.bodySmall,
                ),
                subtitle: Text(
                  'size: ${cartItem['Size']}',
                ),
              );
            }));
  }
}
import 'package:flutter/material.dart';
import 'package:trade_app/global_variables.dart';
import 'package:trade_app/product_card.dart';
import 'package:trade_app/product_details.dart';

class ProductList extends StatefulWidget {
  const ProductList({super.key});

  @override
  State<ProductList> createState() => _ProductListState();
}

class _ProductListState extends State<ProductList> {
  final List<String> filters = const ['All', 'Adidas', 'Nike', 'Bata'];

  late String selectedFilter;
  @override
  void initState() {
    super.initState();
    selectedFilter = filters[0];
  }

  @override
  Widget build(BuildContext context) {
    const border = OutlineInputBorder(
        borderSide: BorderSide(
          color: Color.fromRGBO(
            225,
            225,
            225,
            1,
          ),
        ),
        borderRadius: BorderRadius.horizontal(left: Radius.circular(50)));
    return SafeArea(
      child: Column(
        children: [
          Row(
            children: [
              Padding(
                padding: const EdgeInsets.all(8.0),
                child: Text(
                  'Shoes\nCollection',
                  style: Theme.of(context).textTheme.titleLarge,
                ),
              ),
              const Expanded(
                child: TextField(
                  decoration: InputDecoration(
                    hintText: 'Search',
                    prefixIcon: Icon(
                      Icons.search,
                    ),
                    border: border,
                    enabledBorder: border,
                    focusedBorder: border,
                  ),
                ),
              ),
            ],
          ),
          SizedBox(
            height: 120,
            child: ListView.builder(
                itemCount: filters.length,
                scrollDirection: Axis.horizontal,
                itemBuilder: (context, index) {
                  final filter = filters[index];
                  return Padding(
                    padding: const EdgeInsets.symmetric(horizontal: 8),
                    child: GestureDetector(
                      onTap: () {
                        setState(() {
                          selectedFilter = filter;
                        });
                      },
                      child: Chip(
                        backgroundColor: selectedFilter == filter
                            ? Theme.of(context).colorScheme.primary
                            : const Color.fromRGBO(245, 247, 249, 1),
                        side: const BorderSide(
                            color: Color.fromRGBO(245, 247, 249, 1)),
                        padding: const EdgeInsets.symmetric(
                            horizontal: 20, vertical: 15),
                        shape: RoundedRectangleBorder(
                            borderRadius: BorderRadius.circular(30)),
                        label: Text(filter),
                        labelStyle: const TextStyle(fontSize: 16),
                      ),
                    ),
                  );
                }),
          ),
          Expanded(
            child: ListView.builder(
                itemCount: products.length,
                itemBuilder: (context, index) {
                  final product = products[index];
                  return GestureDetector(
                    onTap: () {
                      Navigator.of(context).push(
                        MaterialPageRoute(
                          builder: (context) {
                            return ProductDetailsPage(product: product);
                          },
                        ),
                      );
                    },
                    child: Expanded(
                      child: ProductCard(
                        title: product['title'] as String,
                        price: double.tryParse("$product['price]") ?? 00,
                        image: product['imageUrl'] as String,
                        backgroundColor: index.isEven
                            ? const Color.fromRGBO(216, 214, 253, 1)
                            : const Color.fromRGBO(245, 247, 249, 1),
                      ),
                    ),
                  );
                }),
          )
        ],
      ),
    );
  }
}
import 'package:flutter/material.dart';

class ProductCard extends StatelessWidget {
  final image;
  final String title;
  final double price;
  final Color backgroundColor;

  const ProductCard(
      {super.key,
      required this.image,
      required this.price,
      required this.title,
      required this.backgroundColor,
      TextStyle? style});

  @override
  Widget build(BuildContext context) {
    return Container(
      margin: const EdgeInsets.all(20),
      padding: const EdgeInsets.all(16),
      decoration: BoxDecoration(
        color: backgroundColor,
        borderRadius: BorderRadius.circular(20),
      ),
      child: Padding(
        padding: const EdgeInsets.all(8.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              title,
              style: Theme.of(context).textTheme.titleMedium,
            ),
            const SizedBox(height: 5),
            Text(
              '\$$price',
              style: Theme.of(context).textTheme.bodySmall,
            ),
            const SizedBox(height: 5),
            Center(
              child: Image(
                image: AssetImage(image),
                height: 100,
              ),
            )
          ],
        ),
      ),
    );
  }
}
import 'package:flutter/material.dart';

import 'package:trade_app/home_page.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Shopping App',
      theme: ThemeData(
        fontFamily: 'Roboto',
        colorScheme: ColorScheme.fromSeed(
          seedColor: const Color.fromRGBO(254, 206, 1, 1),
          primary: const Color.fromRGBO(254, 206, 1, 1),
        ),
        appBarTheme: const AppBarTheme(
            titleTextStyle: TextStyle(fontSize: 20, color: Colors.green)),
        inputDecorationTheme: const InputDecorationTheme(
            hintStyle: TextStyle(fontWeight: FontWeight.bold, fontSize: 16),
            prefixIconColor: Color.fromRGBO(119, 119, 119, 1)),
        textTheme: const TextTheme(
            titleLarge: TextStyle(fontWeight: FontWeight.bold, fontSize: 35),
            titleMedium: TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
            bodySmall: TextStyle(fontWeight: FontWeight.bold, fontSize: 16)),
        useMaterial3: true,
      ),
      home: const HomePage(),
    );
  }
}
final products = [
  {
    'id': '0',
    'title': 'Men\'s Nike Shoes',
    'price': 44.32,
    'company': 'Adidas',
    'imageUrl': 'assets/images/shoes_1.png',
    'sizes': [8, 9, 11],
  },
  {
    'id': '1',
    'title': 'Men\'s Nike Shoes',
    'price': 40.32,
    'company': 'Nike',
    'imageUrl': 'assets/images/shoes_2.png',
    'sizes': [9, 10, 11],
  },
  {
    'id': '2',
    'title': 'Men\'s Nike Shoes',
    'price': 44.32,
    'company': 'Gucci',
    'imageUrl': 'assets/images/shoes_3.png',
    'sizes': [8, 9, 11],
  },
  {
    'id': '3',
    'title': 'Men\'s Nike Shoes',
    'price': 39.32,
    'company': 'Bata',
    'imageUrl': 'assets/images/shoes_4.png',
    'sizes': [8, 9, 10, 11],
  },
  {
    'id': '4',
    'title': 'Men\'s Nike Shoes',
    'price': 45,
    'imageUrl': 'assets/images/shoes_5.png',
    'company': 'versace',
    'sizes': [11, 12],
  }
];
final cart = [
  {
    'id': '3',
    'title': 'Jordan Shoes',
    'price': '420.69',
    'imageUrl': 'assets/images/shoes_4.png',
    'size': [10],
  }
];
# flutter
