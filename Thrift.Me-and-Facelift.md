# Thrift.Me and Facelift

<!-- MarkdownTOC autolink=true -->

- [Thrift.me](#thriftme)
  - [https://gsep.daimler.com/stash/pages/NTGSHUS/ThriftMeDoc/master/browse/index.html](#httpsgsepdaimlercomstashpagesntgshusthriftmedocmasterbrowseindexhtml)
    - [Create and registering srevices on servers](#create-and-registering-srevices-on-servers)
      - [Implement service](#implement-service)
      - [Registration of srevices](#registration-of-srevices)
      - [deferred functions](#deferred-functions)
    - [Creating and using clients](#creating-and-using-clients)
      - [client creation](#client-creation)
      - [waiting for connection](#waiting-for-connection)
      - [function calls](#function-calls)
    - [Event Distribution](#event-distribution)
    - [Authorization Level](#authorization-level)
- [Facelift](#facelift)
  - [](#)

<!-- /MarkdownTOC -->

# Thrift.me

## https://gsep.daimler.com/stash/pages/NTGSHUS/ThriftMeDoc/master/browse/index.html
- Function Modifiers
  - oneway
  - event: a function called by server and received by connected clients (callback function). The Client have to provide an implementation of the function.
  - deferred: only influence the generation of server-side code.
  - propertygs
  - viral, immuen

### Create and registering srevices on servers

#### Implement service

From a service with the name `$(ServiceName)` the Thrift.Me the compiler will create a bunch of interfaces:

The interface `I$(ServiceName)Client`, which contains all functions exactly as they were defined in the IDL. This kind of interface is used by clients.
The interface `I$(ServiceName)Server`, which also adds the const TCallContext& context parameter to each function.
Servers that want to provide a Thrift.Me service have to implement the `I$(ServiceName)Server` interface and set up a `$(ServiceName)Processor`.

```
class CCEServer : public CentralControlElementProcessor
{
  ...
  // CCE Function implementations.
  // All functions which are defined in ICentralControlElementServer have to be implemented.
};
// Create the CCE objects implementation
shared_ptr<CCEServer> cceServer(new CCEServer(broker));
// shared_ptr usage here is intentional and internal structures of Thrift.Me assume that the instance is wrapped by a shared_ptr object and might not work otherwise.
```

#### Registration of srevices

```
cceServer->registerService(0);	// 0 is service_id
```

- clients on the same device can connect ot it using URI: tcp://127.0.0.1:9100/#0

#### deferred functions

- Deferred function in IDL:
```
deferred list<ExampleEntry> getItems(1:int32 offset, 2:int32 length) throws (1:ListException x);
```
  - generated function
  ```
  void getItems(const int32_t offset, const int32_t length,
              const ::thrift::TDeferredResponse& responseHandle,
              const ::thrift::transport::TCallContext& context);
  // ...           
  respondTo_getItems(responseHandle, returnValue);
  ```
- normal (non-deferred) function in IDL:
```
list<ExampleEntry> getItems(1:int32 offset, 2:int32 length) throws (1:ListException x);
```
  - generated function
  ```
  std::list getItems(const int32_t offset, const int32_t length,
                   const ::thrift::transport::TCallContext& context)
  ```

### Creating and using clients

#### client creation

```
shared_ptr<testServiceClient> client = testServiceClient::createClient(broker, "tcp://127.0.0.1:9100/#0");
client->dispose(); // must be called if a client object is no longer needed.
```

#### waiting for connection
The connections are established asynchronously in the background.

1. `client->getTransport()->waitForConnection()`
2. wait for `connectionStatusChanged` callback.

#### function calls

1. `retValue = client->function(param);`
2. asynchronous function call
  ```
  shared_ptr<func_asyncResult> ar = clinet->send_function(param);
  // ...
  retValue = recv_function(ar);
  ```
3. asyncronous func call with callback
  ```
  client->send_function(
  	param,
  	std::tr1::bind(...),
  	std::tr1::bind(...),
  	userState);
  ```


### Event Distribution

- 3 concepts of distribution of events:
  - Singlecast
  - Multicast
  - Broadcast: the event is sent to all connected clients. 
    - Default mechanism of Thrift.Me for each event defined in IDL.
    - For each event in IDL, an event distribution function with the same event name will be generaged by Thrift.Me compiler into the service processor.
      - Remark: the event will only be broadcasted to clients when the server is configured to distribute events.

### Authorization Level

```
/**
 * @authLevel 2
 */
void functionThatNeedsAuth();
```


# Facelift

## 

































