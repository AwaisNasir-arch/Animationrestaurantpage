import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      debugShowCheckedModeBanner: false,
      home: MenuPage(),
    );
  }
}

class MenuPage extends StatelessWidget {
  const MenuPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Restaurant Menu'),
      ),
      body: SafeArea(
        child: SingleChildScrollView(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            children: const [
              FoodItem(
                name: 'Chicken Burger',
                price: 'Rs. 450',
                imageUrl: 'https://images.unsplash.com/photo-1568901346375-23c9450c58cd?w=300',
              ),
              Divider(height: 30),
              FoodItem(
                name: 'Pizza',
                price: 'Rs. 850',
                imageUrl: 'https://images.unsplash.com/photo-1574071318508-1cdbab80d002?w=300',
              ),
            ],
          ),
        ),
      ),
    );
  }
}

class FoodItem extends StatefulWidget {
  final String name;
  final String price;
  final String imageUrl;

  const FoodItem({
    super.key,
    required this.name,
    required this.price,
    required this.imageUrl,
  });

  @override
  State<FoodItem> createState() => _FoodItemState();
}

class _FoodItemState extends State<FoodItem> {
  bool _crossFadeStateShowFirst = true;

  void _crossFade() {
    setState(() {
      _crossFadeStateShowFirst = !_crossFadeStateShowFirst;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Row(
      crossAxisAlignment: CrossAxisAlignment.center,
      children: [
        // Using AnimatedCrossFade with balanced dimensions
        GestureDetector(
          onTap: _crossFade,
          child: AnimatedCrossFade(
            duration: const Duration(milliseconds: 500),
            sizeCurve: Curves.bounceOut,
            crossFadeState: _crossFadeStateShowFirst
                ? CrossFadeState.showFirst
                : CrossFadeState.showSecond,
            firstChild: ClipRRect(
              borderRadius: BorderRadius.circular(10),
              child: Image.network(
                widget.imageUrl,
                width: 80,
                height: 80,
                fit: BoxFit.cover,
              ),
            ),
            secondChild: ClipRRect(
              borderRadius: BorderRadius.circular(16),
              child: Image.network(
                widget.imageUrl,
                width: 240,
                height: 240,
                fit: BoxFit.cover,
              ),
            ),
          ),
        ),
        const SizedBox(width: 16),
        Expanded(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text(
                widget.name,
                style: const TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
              ),
              Text(
                widget.price,
                style: const TextStyle(fontSize: 16, color: Colors.grey),
              ),
              const SizedBox(height: 6),
              ElevatedButton(
                onPressed: () {
                  ScaffoldMessenger.of(context).showSnackBar(
                    const SnackBar(
                      content: Text('Order successfully!'),
                      duration: Duration(seconds: 5),
                    ),
                  );
                },
                child: const Text('Order'),
              ),
            ],
          ),
        ),
      ],
    );
  }
}
