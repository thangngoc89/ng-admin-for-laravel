ng-admin-for-laravel
====================

This help you can create a quick AngularJS backend with ng-admin for Laravel
#How to ?
- Require Laravel API Handler at https://github.com/marcelgwerder/laravel-api-handler
- Create a RESTful Controller with Laravel http://laravel.com/docs/4.2/controllers#restful-resource-controllers
Ex: 'php artisan controller:make UserController'
- Place this function in the class
- 'private function passParams($table_name){
        //ng-admin ?_sort=name&_sortDir=DESC&page=1&per_page=30
        //API _fields, _with, _sort, _limit, _offset

        //Print out all table's columns
        $columns = Schema::getColumnListing($table_name);
        $params = [
            '_limit'    => 10,
            '_offset'   => 0,
        ];
        // If have _sort is a valid column
        if (in_array(Input::get('_sort'), $columns))
        {
            // Default sortDir is ASC
            $sortDir = (!empty(Input::get('_sortDir'))) ? Input::get('_sortDir') : 'ASC';
            // ASC --> null   DESC --> -
            $sort = ($sortDir === 'ASC' ) ? '' : '-';
            $sort .= Input::get('_sort');
            $params ['_sort'] = $sort;
        }
        if (!empty(Input::get('page')) && !empty(Input::get('per_page')))
        {
            $params['_limit'] = Input::get('per_page');
            $params['_offset'] = (Input::get('page') - 1)*Input::get('per_page');
        }
        if (!empty(Input::get('q')) && Input::get('q') != '{}')
        {
            $query = json_decode(Input::get('q'));
            $params['_q'] = $query->query;
        }

        foreach ($columns as $column)
        {
            if (!empty(Input::get($column)))
            {
                echo $column.'<br />';
                $params[$column] = Input::get($column);
            }
        }
        $params ['_config'] = 'meta-filter-count';
        return $params;
    }'
