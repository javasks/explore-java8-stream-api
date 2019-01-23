# Exploring Java SE 8 Stream API

The Java API designers are updating the API with a new abstraction called Stream that lets you process data in a declarative way. Furthermore, streams can leverage multi-core architectures without you having to write a single line of multithread code.

Before we explore in detail what you can do with streams, let’s take a look at an example so you have a sense of the new programming style with Java SE 8 streams. Let’s say we need to find all transactions of type grocery and return a list of transaction IDs sorted in decreasing order of transaction value.

### 1. JAVA SE 7 Style
```
List<Transaction> groceryTransactions = new ArrayList<>();
for(Transaction t: transactions){
  if(t.getType() == Transaction.GROCERY){
    groceryTransactions.add(t);
  }
}
Collections.sort(groceryTransactions, new Comparator(){
  public Integer compare(Transaction t1, Transaction t2){
    return t2.getValue().compareTo(t1.getValue());
  }
});
List<Integer> transactionIds = new ArrayList<>();
for(Transaction t: groceryTransactions){
  transactionsIds.add(t.getId());
}
```
### 2. JAVA SE 8 STREAM API Style
```
List<Integer> transactionsIds = 
    transactions.stream()
                .filter(t -> t.getType() == Transaction.GROCERY)
                .sorted(comparing(Transaction::getValue).reversed())
                .map(Transaction::getId)
                .collect(toList());             