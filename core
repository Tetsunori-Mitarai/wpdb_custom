<?php

class data_hoge
{    
    const NAME_TABLE = 'hoge';
    
    protected $_id;
    protected $_id_hoge;    
    
    protected $_updated;
        
    public function __construct($id_hoge)
    {
        if ( empty($id_hoge) )
        {
            throw new Exception('IDが空でした。KEYが無いためDBから取得できません。');
        }
        
        global $wpdb;
     
        if( IS_DEBUG ){ $wpdb->show_errors(); }
        
        $row = $wpdb->get_row (
            $wpdb->prepare('SELECT * FROM '.$wpdb->prefix . self::NAME_TABLE .' WHERE id_hoge = %d', $id_hoge),
            OBJECT
        );
        
        if ($row === false)
        {
            if( IS_DEBUG ){ $wpdb->print_error(); }
            
            throw new Exception('データの新規追加に失敗。');
        }
        
        if ($row <= 0)
        {
            $this->_id_hoge = $id_hoge;
            return;
        }
        
        $this->_id                  = $row->id;
        $this->_id_hoge             = $row->id_hoge;
    }
    
    public function getID() { return $this->_id; }
    
    public function setID($value){ $this->_id = $value; }
    
    public function insert()
    {
        global $wpdb;
        
        if( IS_DEBUG ){ $wpdb->show_errors(); }
        
        $row = $wpdb->insert(
            $wpdb->prefix . self::NAME_TABLE,
            array( // SET
                'id_hoge' => $this->_id_hoge,
            
                'updated' => date('Y-m-d H:i:s')
            ), 
            array(
                '%d',
                
                '%s'
            )
        );
        
        if ($row <= 0 || $row === false)
        {
            if( JNCAP_GENERAL_MANAGER_DEBUG_MODE ){ $wpdb->print_error(); }
            
            throw new Exception('データの新規追加に失敗。LastResult:'.$wpdb->last_result);
        }
    }     
    
    public function update()
    {
        global $wpdb;
     
        if( IS_DEBUG ){ $wpdb->show_errors(); }
        
        $row = $wpdb->get_row (
            $wpdb->prepare('SELECT * FROM '.$wpdb->prefix . self::NAME_TABLE .' WHERE id = %d', $this->_id),
            OBJECT
        );
        
        if (empty($row))
        {
            $this->insert();
            
            return;
        }
        
        $row = $wpdb->update(
            $wpdb->prefix .$wpdb->prefix . self::NAME_TABLE,
            array( // SET
                'id'      => $this->_id,
                'id_hoge' => $this->_id_hogee,
                
                'updated' => date('Y-m-d H:i:s')
            ), 
            array(
                'id' => $this->_id
            ),
            array(
                '%d',
                '%d',
                
                '%s'
            ),
            array(
                '%d'
            )
        );
        
        if ($row <= 0 || $row === false)
        {
            if( JNCAP_GENERAL_MANAGER_DEBUG_MODE ){ $wpdb->print_error(); }
            
            throw new Exception('データの更新に失敗。LastResult:'.$wpdb->last_result);
        }
    }
    
    public static function create()
    {
        // テーブルの有無をチェックして無ければ作成
        global $wpdb;
        
        $table_name = $wpdb->prefix .self::NAME_TABLE;
        
        $wpdb->get_row('SHOW TABLES FROM ' . DB_NAME . ' LIKE \'' . $table_name . '\'');
        
        if( $wpdb->num_rows != 1 ){ 
            //テーブル作成
            $charset_collate = $wpdb->get_charset_collate();

            $query = "CREATE TABLE $table_name (
                id      int(1) NOT NULL AUTO_INCREMENT,
                id_hoge int(1),
                
                created TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
                updated TIMESTAMP,
                deleted TIMESTAMP,
                
                PRIMARY KEY id (id)
            ) $charset_collate;";
            
            dbDelta( $query );
        }
    }
}
