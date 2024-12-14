### Larvel	

1. laravel-admin 后台redis报错：vendor/laravel-admin-ext/redis/src

   ```
   public function getConnection($connection = null)
   {
       if ($connection) {
           $this->connection = $connection;
       }
       redis::setDriver('predis'); 
       return Redis::connection($this->connection);
   }
   ```

   