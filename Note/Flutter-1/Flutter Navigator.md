In general, one Flutter project has one MaterialApp, one MaterialApp has one Navigator, and many Scaffold. Scaffold means page in flutter. so Navigator control the route between Scaffolds in one MaterialApp.

So Navigator.of(context) means find the (state of) Navigator in the MaterialApp by the context of Scaffold.