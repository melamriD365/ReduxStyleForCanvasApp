# **Dispatch Component for Redux-Style State Management**

## **Overview**

This **Dispatch Component** is a flexible and reusable tool for implementing **Redux-style state management** in Power Apps Canvas Apps. With the introduction of **advanced property types** such as **Event**, **Function**, and **Action**, this component leverages these new capabilities to centralize state updates and side effects, making your Canvas Apps more maintainable and scalable.

---

## **Features**

1. **Centralized State Updates**:
   - The component allows you to dispatch actions to modify the global store (`gblStore`) in a controlled, centralized manner.

2. **Side Effects Management**:
   - Execute side effects like sending emails or triggering notifications alongside state updates.

3. **New Property Types**:
   - Uses the new **Event properties** (`OnSuccess`, `OnError`) to notify the host app of success or failure during dispatch execution.
   - Accepts **input properties** (`gblStoreIn`, `DispatchAction`, `DispatchPayload`) to dynamically process data.
   - Outputs the updated store through an **output property** (`gblStoreOut`).

4. **Error Handling**:
   - Automatically triggers the `OnError` event if an error occurs during the dispatch process.

---

## **Component Properties**

### **Input Properties**

| **Name**          | **Type**   | **Description**                                                              |
|-------------------|------------|------------------------------------------------------------------------------|
| `gblStoreIn`      | **Text** | The current global store in JSON format, representing the app’s state.       |
| `DispatchAction`  | **Text**   | The action to be executed (e.g., `"MARK_SHIPPED"`).                          |
| `DispatchPayload` | **Text**   | A JSON payload containing additional data required for the action.           |

---

### **Output Properties**

| **Name**       | **Type**   | **Description**                                                              |
|----------------|------------|------------------------------------------------------------------------------|
| `gblStoreOut`  | **Record** | The updated global store after the dispatch action has been executed.        |

---

### **Event Properties**

| **Name**       | **Type**   | **Description**                                                              |
|----------------|------------|------------------------------------------------------------------------------|
| `OnSuccess`    | **Event**  | Triggered when the dispatch action completes successfully.                   |
| `OnError`      | **Event**  | Triggered when an error occurs during the dispatch action.                   |

---

## **How It Works**

### **Dispatch Logic**

The component processes actions using the following logic:

1. **Input Properties**:
   - `gblStoreIn` provides the current app state (global store).
   - `DispatchAction` specifies the type of action to execute (e.g., `"MARK_SHIPPED"`).
   - `DispatchPayload` provides additional data for the action (e.g., the `orderId`).

2. **Action Execution**:
   - The `Switch` statement matches the `DispatchAction` to predefined logic.
   - Example: `"MARK_SHIPPED"` updates the order status to `"Shipped"` and sends an email notification.

3. **Side Effects**:
   - For `"MARK_SHIPPED"`, an email notification is sent using `Office365Outlook.SendEmail`.

4. **Output**:
   - The updated store is set to `gblStoreOut`.

5. **Error Handling**:
   - If an error occurs, the `OnError` event is triggered.
   - On successful execution, the `OnSuccess` event is triggered.

---

### **Dispatch Example**

Here’s an example of how to use the component in a Canvas App:

1. **Setup**:
   - Insert the `Dispatch` component into your screen.
   - Bind the input properties.

2. **Calling Dispatch**:
   ```powerapps
   // Example: Mark an order as shipped
   DispatchComponent.DispatchAction = "MARK_SHIPPED";
   DispatchComponent.DispatchPayload = JSON({ orderId: 1234 });
   DispatchComponent.gblStoreIn = gblStore;
