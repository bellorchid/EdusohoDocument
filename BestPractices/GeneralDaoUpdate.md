
<!--
create time: 2018-03-13 13:59:27
Author: <TODO: 请写上你的名字>

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

## GeneralDao update 使用风险

* 在`GeneralDao`中的update方法中，提供了两种用法：
    * 通过默认主键id更新单条数据
    * 通过一组条件去批量更新数据

* 三段代码如下
    * update

    ```php
    public function update($identifier, array $fields)
    {
        if (empty($identifier)) {
            return 0;
        }

        if (is_numeric($identifier) || is_string($identifier)) {
            return $this->updateById($identifier, $fields);
        }

        if (is_array($identifier)) {
            return $this->updateByConditions($identifier, $fields);
        }

        throw new DaoException('update arguments type error');
    }
    ```

    * updateByConditions

    ```php
    protected function updateByConditions(array $conditions, array $fields)
    {
        $builder = $this->createQueryBuilder($conditions)
            ->update($this->table, $this->table);

        foreach ($fields as $key => $value) {
            $builder
                ->set($key, ':'.$key)
                ->setParameter($key, $value);
        }

        return $builder->execute();
    }
    ```

    * createQueryBuilder

    ```php
    protected function createQueryBuilder($conditions)
    {
        $conditions = array_filter(
            $conditions,
            function ($value) {
                if ('' === $value || null === $value) {
                    return false;
                }

                if (is_array($value) && empty($value)) {
                    return false;
                }

                return true;
            }
        );

        $builder = $this->getQueryBuilder($conditions);
        $builder->from($this->table(), $this->table());

        $declares = $this->declares();
        $declares['conditions'] = isset($declares['conditions']) ? $declares['conditions'] : array();

        foreach ($declares['conditions'] as $condition) {
            $builder->andWhere($condition);
        }

        return $builder;
    }
    ```

* 看到这里我们不难发现，当我们采取conditions方式去更新数据的时候，如果参数的key为空或者`NULL`,而我们又没有对conditions做业务上的校验，就会出现一些查询条件丢失的情况，直接导致更新数据的范围和预想的范围不一致，极端情况下，如果所有参数全部为空，整张表的数据都会被更新。

* 最后，提醒各位开发者，updateByConditions模式慎用！！在无法保证参数的稳定性的情况下，会引起严重的事故 ，下面建议使用的要求
    * 在Dao对`通过查询条件更新的方式`进行一层方法封装，方法内一定对符合当前查询条件的参数做严格过滤，参数不能多不能少不能为空
    * 在业务层对参数的传递上，要做好为空判断，遇到异常数据抛异常或者直接不执行