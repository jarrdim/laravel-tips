# laravel-tips


CREATING CONSTANT FILE

    1.Create a new file called 'constants.php' in your Laravel project's config directory
      (you may need to create the config directory if it doesn't exist).
  2. Open the constants.php file and define your constants using the define function. Here's an example:


    <?php
      return [
          'PERMISSIONS' => [
              'view_dashboard',
              'view_category',
              'add_category',
              
          ],
      ];



  1. Save the constants.php file.

  2. In your Laravel application, open the config/app.php file.

  3. Find the providers array in the app.php file.

  4. Add the following line to the providers array:
     
    'providers' => [
    // Other providers...

    /*
     * Custom Configuration Files
     */
    App\Providers\ConstantsServiceProvider::class,
    ],
  
1. Save the app.php file.
2. In your Laravel project's app/Providers directory, create a new file called
   ConstantsServiceProvider.php.

3. Open the 'ConstantsServiceProvider.php' file and add the following code:
   
            <?php
              namespace App\Providers;
              use Illuminate\Support\ServiceProvider;
              class ConstantsServiceProvider extends ServiceProvider
              {
                  /**
                   * Register any application services.
                   *
                   * @return void
                   */
                  public function register()
                  {
                      config([
                          'constants' => require config_path('constants.php'),
                      ]);
                  }
              }


1. Save the ConstantsServiceProvider.php file.
2. Run the following command in your terminal to reload the configuration:
   
         php artisan config:cache

3. After completing these steps, you should be able to access the PERMISSIONS constant
4. using the config('constants.PERMISSIONS') syntax in your Laravel application.
  
        $permissions = config('constants.PERMISSIONS');








SERVICE CLASSES:

    php artisan make:provider CustomServiceProvider
    
This command will generate a new file called CustomServiceProvider.php
in the app/Providers directory.

Open the CustomServiceProvider.php file and 
define your custom function in the register() method. For example:

                <?php

                  namespace App\Providers;
                  
                  use Illuminate\Support\ServiceProvider;
                  
                  class CustomServiceProvider extends ServiceProvider
                  {
                      public function register()
                      {
                          $this->app->bind('custom', function () {
                              return new \App\Services\CustomService();
                          });
                      }
                  }


In the example above, we bind the custom service to an instance of the 

CustomService class. You will need to create the CustomService class as well.

In your Laravel project's root directory, navigate to the app directory.

Create a new directory called customeservice within the app directory.

Inside the customeservice directory, create a new PHP file named CustomService.php.

Open the CustomService.php file in a text editor or IDE.

Define your custom service class within the CustomService.php file. Here's an example:

              <?php

                namespace App\CustomService;
                
                class CustomService
                {
                    public function user_can($permission)
                    {
                        // Add your logic here to check the user's permission
                        // and return true or false accordingly
                        
                        // Example logic:
                        if ($permission === 'edit') {
                            return true;
                        } else {
                            return false;
                        }
                    }
                }


Save the CustomService.php file.

Next, open the config/app.php file and add your service provider to the providers array:


            'providers' => [
                // Other providers...
                App\Providers\CustomServiceProvider::class,
            ],


Save the config/app.php file.

You can now access your custom function by resolving the custom service. For example:


                $customService = app('custom');
                
                if ($customService->user_can('edit')) {
                    echo "User can edit.";
                } else {
                    echo "User cannot edit.";
                }



Remember to run the following command to optimize your application's

performance after adding helper functions or service classes:

            php artisan optimize

