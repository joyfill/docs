# Event handling

This guide covers the event callbacks available in Joyfill forms and when they're triggered.

### **Available Events**

| Event | When Triggered | Purpose |
| --- | --- | --- |
| onFieldChange | Field value changes | Track form changes |
| onFocus | Field gains focus | Handle field focus |
| onBlur | Field loses focus | Handle field blur |
| onUpload | File upload requested | Handle file uploads |
| onCapture | Barcode scan requested | Handle barcode scanning |

## **onChange Event**

Triggered when any field value changes in the form.

**Parameters:**

- changes: [Change] - Array of change objects describing what was modified
- document: JoyDoc - The fully updated document with all changes applied

```swift
func onChange(changes: [Change], document: JoyDoc) {        
     if let firstChange = changes.first {
           print(">>>>>>>>onChange", firstChange.change ?? "")
     }
     // The document parameter contains the fully updated form
}
```

![output.gif](Event%20handling/output.gif)

## **onFocus Event**

Triggered when a field receives focus.

**Parameters:**

- event: FieldIdentifier - Information about the current focused field

```swift
func onFocus(event: Joyfill.FieldIdentifier) {
     print("Field focused: \(event.fieldID)")
}
```

## **onBlur Event**

Triggered when a field loses focus.

**Parameters:**

- event: FieldIdentifier - Information about the field that lost focus

```swift
func onBlur(event: Joyfill.FieldIdentifier) {
     print("Field blurred: \(event.fieldID)")
}
```

## **onUpload Event**

Triggered when a file upload is requested for image or file fields.

**Parameters:**

- event: UploadEvent - Upload event details

**Properties Available:**

- fieldEvent: FieldIdentifier - The field requesting upload
- multi: Bool - Whether multiple files are allowed
- uploadHandler: ([String]) -> Void - Callback to provide file URLs

```swift
func onUpload(event: UploadEvent) {
        print("📤 Upload requested for field: \(event.fieldEvent.fieldID)")
        
        // Option 1: Present a photo picker for the user to choose an image.
        // Option 2: Directly send pre-uploaded image identifiers or URLs to the upload handler.
        
        let exampleImageURL = "https://example.com/uploads/sample-image.jpg"
        
        // Example: sending a direct image URL or an identifier for the upload
        event.uploadHandler([exampleImageURL])
    }
```

## **onCapture Event**

Triggered when camera capture is requested for Barcode cell.

**Parameters:**

- event: CaptureEvent - Capture event details

**Properties Available:**

- fieldEvent: FieldIdentifier - The field requesting capture
- captureHandler: (ValueUnion) -> Void - Callback to provide captured content

```swift
    func onCapture(event: Joyfill.CaptureEvent) {
        print("📷 Capture requested for field: \(event.fieldEvent.fieldID)")
        print("User can open a barcode scanner to capture the code.")
        event.captureHandler(.string("Scan Button Clicked"))
    }
```

## **onError Event**

- Used to listen to errors during document processing.
- error: JoyfillError — details about the failure.
- Error types include:
    - schemaValidationError — Document schema validation failures
    - schemaVersionError — SDK and document version compatibility issues

```swift
func onError(error: Joyfill.JoyfillError) {
        switch error {
        case .schemaValidationError(let schemaError):
            print("❌ Schema Error: \(schemaError)")
        case .schemaVersionError(let versionError):
            print("❌ Schema Error: \(versionError)")
        }
        print("Error occurred: \(error)")
    }
```