# MyTreeMap
Этот код реализует бинарное дерево поиска в виде класса MyTreeMap, который является аналогом TreeMap из Java Collections. Вот основная суть:
public class Homework3 {
    public static class MyTreeMap<K, V> {

        private class Node {
            public K key;
            public V value;

            public Node left;
            public Node right;

            protected Node(K key, V value) {
                this.key = key;
                this.value = value;
            }
        }

        private int size;
        private Node root;

        private Node findNode(Object key) {
            Comparable<K> cKey = (Comparable<K>) key;
            Node node = this.root;

            while(node != null) {
                int cmp = cKey.compareTo(node.key);
                if(cmp < 0) {
                    node = node.left;
                } else if(cmp > 0) {
                    node = node.right;
                } else {
                    return node;
                }
            }
            return null;
        }

        private V putHelper(K key, V value, Node node) {
            Comparable<K> cKey = (Comparable<K>) key;
            int cmp = cKey.compareTo(node.key);
            if(cmp < 0) {
                if(node.left == null) {
                    node.left = new Node(key, value);
                    this.size++;
                    return null;
                }
                return this.putHelper(key, value, node.left);
            }

            if(cmp > 0) {
                if(node.right == null) {
                    node.right = new Node(key, value);
                    this.size++;
                    return null;
                }
                return this.putHelper(key, value, node.right);
            }

            V oldValue = node.value;
            node.value = value;
            return oldValue;
        }

        public V put(K key, V value) {
            if(this.root == null) {
                this.root = new Node(key, value);
                this.size++;
                return null;
            }

            return this.putHelper(key, value, this.root);
        }

        public V get(Object key) {
            Node node = findNode(key);
            if(node == null) return null;
            return node.value;
        }

        private Node removeHelper(Object key) {
            Node child = this.findNode(key);
            Node parent = this.findParent(key);

            if(child.left == null && child.right == null) {
                if(child == parent.left) {
                    parent.left = null;
                }
                if(child == parent.right) {
                    parent.right = null;
                }
                this.size--;
                return child;
            }
            if(child.left == null) {
                if(child == parent.left) {
                    parent.left = child.right;
                }
                if(child == parent.right) {
                    parent.right = child.right;
                }
                if(child == parent) {
                    child.key = child.right.key;
                    child.value = child.right.value;
                    child.right = null;
                }
                this.size--;
                return child;
            }
            if(child.right == null) {
                if(child == parent.left) {
                    parent.left = child.left;
                }
                if(child == parent.right) {
                    parent.right = child.left;
                }
                if(child == parent) {
                    child.key = child.left.key;
                    child.value = child.left.value;
                    child.left = null;
                }
                this.size--;
                return child;
            }

            Node preemnik = this.findSmallest(child.right);
            removeHelper(preemnik.key);
            child.key = preemnik.key;
            child.value = preemnik.value;

            return child;
        }

        private Node findSmallest(Node node) {
            if(node.left == null) {
                return node;
            }
            return this.findSmallest(node.left);
        }

        public V remove(Object key) {
            V oldValue = this.get(key);
            if(oldValue == null) {
                return null;
            }

            if(this.size == 1) {
                this.root = null;
                this.size--;
                return oldValue;
            }

            this.removeHelper(key);

            return oldValue;
        }

        private Node findParent(Object key) {
            Comparable<K> cKey = (Comparable<K>) key;
            Node child = this.root;
            Node parent = this.root;
            while(child != null) {
                int cmp = cKey.compareTo(child.key);
                if(cmp < 0) {
                    parent = child;
                    child = child.left;
                } else if(cmp > 0) {
                    parent = child;
                    child = child.right;
                } else {
                    return parent;
                }
            }
            return null;
        }

        private void LER(Node node) {
            if(node.left != null) {
                this.LER(node.left);
            }
            System.out.println(node.value);
            if(node.right != null) {
                this.LER(node.right);
            }
        }

        public void printTree() {
            if(this.root == null) {
                System.out.println("Tree is empty");
                return;
            }
            System.out.println("Root = " + this.root.value);
            this.LER(this.root);
        }
    }

    public static void main(String[] args) {
        // System.out.println("Hello, World!");

        MyTreeMap<Integer, String> myTreeMap = new MyTreeMap<>();
        myTreeMap.put(100, "100");
        myTreeMap.put(80, "80");
        myTreeMap.put(130, "130");
        myTreeMap.put(50, "50");
        myTreeMap.put(90, "90");
        myTreeMap.put(110, "110");
        myTreeMap.put(170, "170");
        myTreeMap.put(20, "20");
        myTreeMap.put(60, "60");
        myTreeMap.put(85, "85");
        myTreeMap.put(120, "120");
        myTreeMap.put(140, "140");

        myTreeMap.printTree();

        myTreeMap.remove(100);

        myTreeMap.printTree();

        myTreeMap.remove(100);

        myTreeMap.printTree();
    }
}
