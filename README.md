```swift
// Entry Object reference in this code snippet
class Entry {
  var title: String
  var body: String
  var timeStamp: NSDate
  
  var kTilte = "titleKey"
  var kBody = "bodyKey"
  var kTimeStamp = "timeStampKey"
  
  init(title: String, body: String = "", timeStamp: NSDate = NSDate()){
    self.title = title
    self.body = body
    self.timeStamp = timeStamp
  }
}
```

```swift
// Entry object controller
class EntryController{
  private let kEntries = "entriesKey"
  
  var entries: [Entry] = []
  
  static let sharedCcontroller = EntryController() // singleton
}
```

Create a Dictionary Representation in the Model to be used when saving to Persist Store

```swift
var dictionaryRepresentation: [String: AnyObject]{
  return [kTitle: self.title, kBody: self.body, kTimeStamp: self.timestamp]
} //The model in this example has three properties: title, body, timeStamp
```

Create a failable initializer in the Model to load from persist store.

```swift
init?(dictionary: [String: AnyObject]){
  guard let title = dictionary[kTitle] as? String,
  let body = dictionary[kBody] as? String,
  let timeStamp = dictionary[kTimeStamp] as? Date else { return nil }
  
  self.title = title
  self.body = body
  self.timeStamp = timeStamp
}
```

# Functions to Save to and Load from persist
### These Functions are created in the model controller

```swift
// To be called anytime changes need to be saved.
func saveToPersistStore(){
  NSUserDefaults.standardUserDefaults().setObject(entrie.map({$0.dictionaryRepresentation}), forKey: kEntries)
}
```

```swift
// To be called when needed to load saved data. Best placed to be called is in the initializer or before the app is loaded.
func loadFromPersistStore(){
  guard let entriesDictionaryArray = NSUserDefaults.standardUserDefaults().objectForKey(kEntries) as? [[String:AnyObject]] else { return }
  self.entries = entriesDictionaryArray.flatMap({Entry(dictionary: $0)})
}
```

