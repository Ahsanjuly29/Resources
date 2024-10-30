# 🌟 Laravel Custom Request Guide

## 📦 Overview
In this guide, we'll explore how to create custom requests in Laravel. This ensures that your API responses are standardized and easily understandable for users. Let’s dive in! 💪

### 🔧 Steps to Create a Custom Request

1. **Create a Request File** 📝  
   Use the Artisan command to generate a request file. This is your starting point!  
   ```bash
   php artisan make:request Task\TaskCreateRequest
   # or
   php artisan make:request Task\TaskUpdateRequest
   ```

2. **Authorization Method** 🔑  
   In the `authorize()` method, simply return `true` to allow the request. This grants access to your request.

3. **Define Validation Rules** ✅  
   In the `rules()` method, specify the rules for your form submission. Here’s an example:  
   ```php
   return [
       'name' => 'required',
       'description' => 'nullable',
       'status' => 'required|in:PENDING,IN_PROGRESS,COMPLETED',
       'due_date' => 'required|date_format:Y-m-d',
   ];
   ```

   **Note:** This concludes the setup for standard Laravel Blade usage. 

### 🌐 API Custom Requests
For API usage, we’ll add some additional features to enhance our requests.

4. **Implement Validation Trait** ⚙️  
   Use the `IsValidRequest` trait to handle validation responses in JSON format. This ensures all API responses are standardized.  
   ```php
   namespace App\Traits;

   use Illuminate\Contracts\Validation\Validator;
   use Illuminate\Http\Exceptions\HttpResponseException;

   trait IsValidRequest
   {
       public function validationData()
       {
           try {
               isApiRequestValidator($this);
               return $this->all();
           } catch (\Exception $e) {
               throw new HttpResponseException(
                   response()->json([
                       'status' => 0,
                       'message' => $e->getMessage(),
                   ], 422)
               );
           }
       }

       public function failedValidation(Validator $validator)
       {
           if ($validator->fails()) {
               throw new HttpResponseException(
                   response()->json([
                       'status' => 0,
                       'message' => $validator->getMessageBag()->toArray(),
                       'errors' => $validator->errors(),
                   ], 422)
               );
           }
       }
   }
   ```

5. **API Request Validation Helper** 🛠️  
   Create a global helper function to check if the API request is valid. This function will be utilized across all requests:  
   ```php
   if (!function_exists('isApiRequestValidator')) {
       function isApiRequestValidator($request)
       {
           try {
               if (!$request->wantsJson()) {
                   throw new Exception('Invalid Request');
               }
           } catch (Exception $e) {
               throw new Exception($e->getMessage());
           }
       }
   }
   ```

### 💡 Conclusion
By following these steps, you can create robust custom requests in Laravel that provide standardized JSON responses for your API. This ensures a seamless experience for your API users! 🎉