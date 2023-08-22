1. Create a field on Account named "Number of Contacts". Populate this field with the number of contacts related to an
account. 


<!-- recentAccountsComponent.cmp -->
<aura:component controller="AccountController">
<aura:attribute name="accounts" type="List" />

<aura:handler init="{!c.doInit}" action="{!c.doInit}" />

<ul>
<aura:iteration items="{!v.accounts}" var="account">
<li>{!account.Name}</li>
</aura:iteration>
</ul>
</aura:component>



2. Build a basic lightning component that can query a list of 10 most recently created accounts and display it using a
lightning app. 

1. **Create Lightning Component:**

Create a new Lightning Component named "RecentAccountsList":

`RecentAccountsList.cmp`:
```html
<aura:component controller="RecentAccountsController">
    <aura:attribute name="recentAccounts" type="Account[]" />
    <aura:handler name="init" value="{!this}" action="{!c.doInit}" />
    
    <h2>Recently Created Accounts</
        
        <ul>
            <aura:iteration items="{!v.recentAccounts}" var="account">
                <li>{!account.Name}</li>
            </aura:iteration>
        </ul>
    </aura:component>
    . **Create Apex Controller:**
    
    Create a new Apex Controller named "RecentAccountsController":
    
    `RecentAccountsController.apxc`:
    ```apex
    public class RecentAccountsController {
    @AuraEnabled
    public static List<Account> getRecentAccounts() {
    return [SELECT Id, Name FROM Account ORDER BY CreatedDate DESC LIMIT 10];
    }
    }
    
    . **Create Lightning Component Controller:**
    
    `RecentAccountsListController.js`:
    ```javascript
    ({
    doInit: function(component, event, helper) {
    var action = component.get("c.getRecentAccounts");
    action.setCallback(this, function(response) {
    var state = response.getState();
    if (state === "SUCCESS") {
    component.set("v.recentAccounts", response.getReturnValue());
    }
    });
    $A.enqueueAction(action);
    }
    })
    ```
    **Create Lightning App:**
    
    Create a new Lightning App named "RecentAccountsApp":
    
    `RecentAccountsApp.app`:
    ```html
    <aura:application >
        <c:RecentAccountsList />
    </aura:application>
    ```
    **Add Lightning Component to Lightning Page:**
    
    Add the "RecentAccountsList" component to a Lightning Page using the Lightning App Builde



3. Make a basic http callout and print the result using system.debug

public class BasicHttpCallout {

public static void makeHttpCallout() {
String endpointUrl = 'https://postman-echo.com/get?foo1=bar1&foo2=bar2';
HttpRequest request = new HttpRequest();
request.setEndpoint(endpointUrl);
request.setMethod('GET');

Http http = new Http();

try {
HttpResponse response = http.send(request);
if (response.getStatusCode() == 200) { 
String responseBody = response.getBody();
System.debug('HTTP Response Body: ' + responseBody);
} else {
System.debug('HTTP Request failed with status code: ' + response.getStatusCode());
}
} catch (Exception ex) {
System.debug('Exception occurred: ' + ex.getMessage());
}
}
}
