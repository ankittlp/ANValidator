# ANValidator

ANValidator helps in easy validation of email, phone number, username based on given ragex rules.

## Installation

### CocoaPods

pod 'ANValidator'

## Usage

`ANValidator` can be used to validate any supported type using a rule defined already or one can extend it to create it own rule. A validate property wrapper project a tuple value with `status` and `statusMessage`.

```swift
import UIKit
import ANValidator

class ViewController: UIViewController {

    @Validate(validation: ValidationRule.username())
    var userName: String?
    
    @Validate(validation: ValidationRule.phoneNumber)
    var phoneNumber: String?
    
    @Validate(validation: ValidationRule.password)
    var password: String?
    
    @Validate(validation: ValidationRule.name)
    var name: String?
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        userName = nil
        if $userName.status {
            print("phone number = \($userName.status) - \($userName.statusMessage) - \(userName)")
        }else {
            print("\($userName.statusMessage)")
        }
        
        phoneNumber = "1234568890"
        if $phoneNumber.status {
            print("phone number = \($phoneNumber.status) - \($phoneNumber.statusMessage) - \(phoneNumber)")
        }else {
            print("\($phoneNumber.statusMessage)")
        }
        
        password = "1234568890"
        if $password.status {
            print("password  = \($password.status) - \($password.statusMessage) - \(password)")
        }else {
            print("\($password.statusMessage)")
        }
        
        name = nil
        if $name.status {
            print("name  = \($name.status) - \($name.statusMessage) - \(name)")
        }else {
            print("\($name.statusMessage)")
        }
    }


}
``` 

In above shown example it is `username` and `phoneNumber` rules have a predefined regex but password has been set to always return true, this can easily be modified to be used as per password policy required. 

```swift
extension ValidationRule where T == String {
    static var password : Self  {
        return self.regex("^(?=.*[0-9])(?=.*[A-Z]).{8,15}$", failedMessage: "Password should be min 8 max 15 char long, should contain one upper case char and a digit.")
    }
    
    static var name: Self {
        
        return .init{ value in
            print("VAlidation Value \(value)")
            return (true,"Success")
        }
    }
}
```
This extension on ValidationRule demonstrate that how we can customise and add in our own rules as required.


