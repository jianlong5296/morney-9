<template>
    <Layout class-prefix="layout">
        <NumberPad :value.sync="record.amount" @submit="saveRecord"/>
        <Tabs :data-source="recordTypeList" :value.sync="record.type"/>
        <div class="createdAt">
            <FormItem field-name="日期"
                      type="date"
                      placeholder="在这里输入日期"
                      :value.sync="record.createdAt"
            />
        </div>
        <div class="notes">
            <FormItem field-name="备注"
                      placeholder="在这里输入备注"
                      :value.sync="record.notes"
            />
        </div>
        <Tags @update:value="record.tags = $event"/>
        <div class="topicon">
            <Icon name="octopus"/>
            <span class="topfont">&nbsp; 章鱼记账</span>
        </div>
    </Layout>
</template>

<script lang="ts">
    import Vue from 'vue';
    import NumberPad from '@/components/Money/NumberPad.vue';
    import FormItem from '@/components/Money/FormItem.vue';
    import Tags from '@/components/Money/Tags.vue';
    import {Component} from 'vue-property-decorator';
    import Tabs from '@/components/Tabs.vue';
    import recordTypeList from '@/constants/recordTypeList';

    @Component({
        components: {Tabs, Tags, FormItem, NumberPad},

    })
    export default class Money extends Vue {
        get recordList() {
            return this.$store.state.recordList;
        }

        recordTypeList = recordTypeList;
        record: RecordItem = {
            tags: [], notes: '', type: '-', amount: 0, createdAt:new Date().toISOString()
        };

        created() {
            this.$store.commit('fetchRecords');

        }

        onUpdateAmount(value: string) {
            this.record.amount = parseFloat(value);
        }

        onUpdateType(value: string) {
            this.record.type = value;
        }

        onUpdateNotes(value: string) {
            this.record.notes = value;
        }

        saveRecord() {
            if (!this.record.tags || this.record.tags.length === 0) {
                return window.alert('请至少选中一个标签');
            }
            this.$store.commit('createRecord', this.record);
            if (this.$store.state.createRecordError === null) {
                window.alert('已保存');
            }
        }
    }
</script>

<style lang="scss" scoped>
    ::v-deep .layout-content {
        display: flex;
        flex-direction: column-reverse;
    }

    .notes {
        padding: 12px 0;
    }
    .topicon{
        flex-grow: 1;
        background: white;
        color: #ffffff;
        font-size: 30px;
        font-weight: bolder;
        border-radius: 10px;
        margin: 6px;
        display: flex;
        justify-content: center;
        align-items: center;
        > .topfont{
            color: black;
        }
    }
</style>
