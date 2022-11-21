# Graph
Data structure for storing and managing complex relationships with a multigraph data structure.

Vertices are represented with integer IDs, which may be positive, 0, or negative.

This data structure initial assumes that 0 means 'no relationship,' and will remove relationships if set to the no_relationship value. Otherwise, use the 'clear' method to remove relationships.

The graph may be directed (A(i,j) may or may not be equal to A(j,i)) or undirected (A(i,j) = A(j,i)).

The graph may be a multigraph, in which case string keys are used to distinguish between different relationships between vertices (A(i,j,key)). If not specified, the key is the empty string "".

Vertex information is not stored within the graph: another structure must contain those data.

```C++
  bygis::Graph G; // creates a directed graph where 0 indicates the absence of a relationship
  
  G.set(0, 1, 30);
  G.set(0, 1, "hi", true, 0.9);
  G.set(3, 1, 0.5);
  
  prnt(G); std::cout << "\n"; // see 'Extra Code', below
```

## Public Members ##

```C++
  G.directed = false;    // change default behavior to that of an undirected graph
  G.directed = true;     // change default behavior to a directed graph
  G.no_relationship = 0; // change the no_relationship value (setting to this value removes the relationship, getting a nonexistent relationship returns this value)
```

## Public Methods ##

For the following:

* i is an integer and represents a vertex ID.
* j is an integer and represents a vertex ID.
* key is a std::string and represents a relationship name or key.
* x is a float and represents a relationship value.

### Construction ###

```C++
  bygis::Graph G;          // constructor for a directed graph.
  bygis::Graph G (dir);    // constructor for a directed graph if dir=true, otherwise undirected.
  bygis::Graph G (dir, x); // constructor for a directed graph if dir=true, otherwise undirected.
```

### Graph Contents ###

```C++
  // contents
  size_t n = G.size();                  // number of vertices represented in G.
  std::set<int> A = G.vertices() const; // set of the IDs of vertices represented in G.
  
  std::set<std::string> K = G.keys() const;     // set of relationship keys represented in G (including "").
  std::set<std::string> K = G.keys(i) const;    // set of relationship keys associated with vertex i.
  std::set<std::string> K = G.keys(i, j) const; // set of relationship keys between vertices i and j.
  
  bool flag = G.contains(i);               // true if a relationship exists with i, same as contains_dir in a directed graph.
  bool flag = G.contains_dir(i);           // true if a relationship exists from i to any vertex.
  bool flag = G.contains_undir(i);         // true if any relationship exists with i.
  bool flag = G.contains(i, j);            // true if a relationship exists before i and j, same as contains_dir in a directed graph.
  bool flag = G.contains_dir(i, j);        // true if a relationship exists from i to j.
  bool flag = G.contains_undir(i, j);      // true if any relationship exists between i and j.
  bool flag = G.contains(i, j, key);       // true if the key relationship exists between i and j, same as contains_dir in a directed graph.
  bool flag = G.contains_dir(i, j, key);   // true if the key relationship exists from i to j.
  bool flag = G.contains_undir(i, j, key); // true if the key relationship exists between i and j.
```

### Neighborhoods ###

```C++
  std::set<int> nbrs = G.nbrs(i);           // neighbors with which i has a relationship, same as nbrs_from in a directed graph.
  std::set<int> nbrs = G.nbrs(i, key);      // vertices with which i has a relationship, same as nbrs_from in a directed graph.
  std::set<int> nbrs = G.nbrs_to(i);        // vertices that have relationships toward i.
  std::set<int> nbrs = G.nbrs_to(i, key);   // vertices that have the key relationship toward i.
  std::set<int> nbrs = G.nbrs_from(i);      // vertices toward which i has a relationship.
  std::set<int> nbrs = G.nbrs_from(i, key); // vertices toward which i has the key relationship.
```

### Relationships ###

If a relationship does not exist, these methods return the no_relationship value.
```C++
  float x = G.get(i, j, key); // get the value of the key relationship between i and j, or no_relationship if it does not exist.
  float x = G.get (i, j);     // as get(i, j, "");
```

Relationships are created if they do not exist, or are instead removed if set to the no_relationship value.
```C++
  G.set(i, j, x);            // set the value of the relationship between i and j as x (creates it if it doesn't exist), same as set_dir in a directed graph.
  G.set_dir(i, j, x);        // set the value of the relationship from i to j as x (creates it if it doesn't exist).
  G.set_undir(i, j, x);      // set the value of the relationship from i to j and from j to i as x (creates it if it doesn't exist).
  
  G.set(i, j, key, x);       // set the value of the key relationship between i and j as x (creates it if it doesn't exist), same as set_dir in a directed graph.
  G.set_dir(i, j, key, x);   // set the value of the key relationship from i to j as x (creates it if it doesn't exist).
  G.set_undir(i, j, key, x); // set to value of the key relationship from i to j and from j to i as x (creates it if it doesn't exist).
```

Besides setting a relationship to the no_relationship value, relationships can also be removed with the 'clear' method.
```C++
  G.clear();                // remove all vertices and relationships from G.
  G.clear(key);             // remove all key relationships from G.
  
  G.clear(i);               // remove vertex i from G, and all associated relationships.
  
  G.clear(i, key);          // remove all key relationships associated with i, same as clear_dir in a directed graph.
  G.clear_dir(i, key);      // remove all key relationships to any vertex from i.
  G.clear_undir(i, key);    // remove all key relationships associated with i.
  
  G.clear(i, j);            // remove all relationships beteen i and j, same as clear_dir in a directed graph.
  G.clear_dir(i, j);        // remove all relationships from i to j.
  G.clear_undir(i, j);      // remove all relationships between i and j.
  
  G.clear(i, j, key);       // remove the key relationship between i and j, same as clear_dir in a directed graph.
  G.clear_dir(i, j, key);   // remove the key relationship from i to j.
  G.clear_undir(i, j, key); // remove the key relationship between i and j.
```

## Extra Code ##

To help with debugging:

```C++
template <typename T>
void prnt ( const std::set<T>& A )
{
	typename std::set<T>::iterator it = A.begin();
	std::cout << "{";
	for ( ; it != A.end(); ++it ) {
		if ( it != A.begin() ) std::cout << " ";
		std::cout << (*it);
	}
	std::cout << "}";
}

void prnt (
	const bygis::Graph& G, int i, int j, const std::string& key )
{
	if ( !G.contains(i,j,key) ) return;
	std::cout << "(" << i << "," << j;
	if ( key.size() > 0 ) std::cout << "," << key;
	std::cout << ")=" << G.get(i,j,key);
}

void prnt ( const bygis::Graph& G, int i, int j )
{
	if ( !G.contains(i,j) ) return;
	int count = 0;
	std::set<std::string> keys = G.keys(i,j);
	std::set<std::string>::iterator it = keys.begin();
	for ( ; it != keys.end(); ++it ) {
		if ( !G.contains(i,j,*it) ) continue;
		if ( count > 0 ) std::cout << "; ";
		prnt(G,i,j,*it);
		++count;
	}
}

void prnt ( const bygis::Graph& G, int i )
{
	int count = 0;
	std::set<int> nbrs = G.nbrs(i);
	std::set<int>::iterator it = nbrs.begin();
	for ( ; it != nbrs.end(); ++it ) {
		if ( !G.contains(i,*it) ) continue;
		if ( count > 0 ) std::cout << "; ";
		prnt(G,i,*it);
		++count;
	}
}

void prnt ( const bygis::Graph& G ) {
	int count = 0;
	std::set<int> V = G.vertices();
	std::set<int>::iterator it = V.begin();
	for ( ; it != V.end(); ++it ) {
		if ( !G.contains(*it) ) continue;
		if ( count > 0 ) std::cout << "; ";
		prnt(G,*it);
		++count;
	}
}
```
