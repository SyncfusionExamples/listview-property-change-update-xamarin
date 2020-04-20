# How to update ListView on property change in Xamarin.Forms (SfListView)

You can update [Grouping](https://help.syncfusion.com/xamarin/listview/grouping?) and [Sorting](https://help.syncfusion.com/xamarin/listview/sorting?) of SfListView on property change by using [LiveDataUpdateMode](https://help.syncfusion.com/cr/xamarin/Syncfusion.DataSource.Portable~Syncfusion.DataSource.DataSource~LiveDataUpdateMode.html?) in Xamarin.Forms.

You can also refer the following article

https://www.syncfusion.com/kb/11419/how-to-update-listview-on-property-change-in-xamarin-forms-sflistview 

**C#**

Adding [GroupDescriptors](https://help.syncfusion.com/cr/xamarin/Syncfusion.DataSource.Portable~Syncfusion.DataSource.DataSource~GroupDescriptors.html?), [SortDescriptors](https://help.syncfusion.com/cr/xamarin/Syncfusion.DataSource.Portable~Syncfusion.DataSource.DataSource~SortDescriptors.html?) and **LiveDataUpdateMode** properties of **DataSource** to ListView.
```
namespace ListViewXamarin
{
    public class Behavior : Behavior<SfListView>
    {
        SfListView listView;
        protected override void OnAttachedTo(SfListView bindable)
        {
            listView = bindable;
            listView.Loaded += Bindable_Loaded;
            base.OnAttachedTo(bindable);
        }
 
        private void Bindable_Loaded(object sender, ListViewLoadedEventArgs e)
        {
            listView.DataSource.LiveDataUpdateMode = LiveDataUpdateMode.AllowDataShaping;
            listView.DataSource.GroupDescriptors.Add(
                new GroupDescriptor()
                {
                    PropertyName = "ContactName",
                    KeySelector = (object obj1) =>
                    {
                        var item = (obj1 as Contacts);
                        return item.ContactName[0].ToString();
                    }
                });
            listView.DataSource.SortDescriptors.Add(
                new SortDescriptor()
                {
                    PropertyName = "ContactName"
                });
        }
        protected override void OnDetachingFrom(SfListView bindable)
        {
            listView.Loaded -= Bindable_Loaded;
            listView = null;
            base.OnDetachingFrom(bindable);
        }
    }
}
```
**C#**

Modifying item on **ListView** item tapped
``` c#
namespace ListViewXamarin
{
    public class ContactsViewModel : INotifyPropertyChanged
    {
        public Command ChangeItem { get; set; }
 
        public ContactsViewModel()
        {
            ChangeItem = new Command(OnChangeItem);
        }
        private void OnChangeItem(object obj)
        {
            if((obj as Syncfusion.ListView.XForms.ItemTappedEventArgs).ItemType == ItemType.GroupHeader)
            {
                return;
            }
            var item = (obj as Syncfusion.ListView.XForms.ItemTappedEventArgs).ItemData as Contacts;
            item.ContactName = "Chan";
        }
    }
}
```
**Output**

![PropertyChangeListview](https://github.com/SyncfusionExamples/listview-property-change-update-xamarin/blob/master/Screenshots/PropertyChanegListview.gif)
