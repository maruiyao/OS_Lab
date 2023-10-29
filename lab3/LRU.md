### LRU
***
基本思路:
1. pra_list_manager中维护两个list：一个是free_list，维护没有被访问的页；一个是busy_list，维护被访问的页
2. 每次添加新页进来都加入到free_list中
3. 每次换页都会优先从free_list中取
4. 每次添加新页或者是触发换页，都会先进行clean_up
5. clean_up会把free_list中的visited为1的页删除，按顺序添加到busy的尾部；busy_list中的visited为0的页删除，添加到free_list的尾部。因为从free_list中取是按照顺序的，所以添加进busy_list中也会是按照访问顺序添加

依旧存在的问题：
这样做只考虑了一个页首次由free->busy时发生的访问，没有考虑如果一个页一直都是busy的情况下，又再次发生访问的情况

为什么要选择这样一个半成品方案？
一开始我打算在page或者是vma中添加一个last_visited_time的标志位，在每一次find_vma发生的时候去更新time。但是这个os在检查swap和page_fault时没有实现对页面的二次访问，并且mm_struct对vma和page的同步更新也存在问题。受限于我的个人水平和时间问题，最终只能选择这个半成品方案完成作业，希望在未来的学习过程中能做出更完善的方案