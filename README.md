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

<aura:component controller="accountsWithContactsClass" implements="flexipage:availableForAllPageTypes" access="global">
<aura:handler name="init" value="{!this}" action="{!c.myAction}"/>
<aura:attribute name="accounts" type="Account[]" />
<table>
<tr>
<td>
<b>Name</b>
</td>
<td>
<b>Industry</b>
</td>
<td>
<b>Contacts</b>
</td>
</tr>
<aura:iteration items="{!v.accounts}" var="accs1" >
<tr>  
<td> {!accs1.Name}  </td>
<td> {!accs1.Industry}  </td>
<!--   <td>   {!accs1.Contacts.lastName}  </td> -->
<table>
<aura:iteration items="{!accs1.Contacts}" var="con1" >
<tr>
<td>{!con1.LastName}</td>
</tr>
</aura:iteration>
</table>
</tr> 
</aura:iteration>                                           
</table>    
</aura:component>








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
