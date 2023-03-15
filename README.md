# Framework Programming - Assignment 2

Muhammad Fatih akbar - 5025201117

---

## The Layout

![image](https://user-images.githubusercontent.com/91055098/225262458-5f4da857-33a3-4aaf-959d-27034428ca90.png)

The UI only consists of this one page, the "Tasklist App Thingy" or homepage. All UI stuff are contained within [MainPage.xaml](/MauiApp2/MainPage.xaml)

## Adding Tasks

![image](https://user-images.githubusercontent.com/91055098/225262923-c4e44b89-94a9-4226-a5ca-fc6094620347.png)

![image](https://user-images.githubusercontent.com/91055098/225263084-c479e2c4-1269-436b-b3b1-7a31853a50ed.png)

Add a task by entering a task in the input field then pressing the add button.

### MainPage.xaml

Bind the text so it passes the text to the ViewModel, and the Add button to trigger the Add RelayCommand in the Viewmodel

```xaml
...
<Entry Placeholder="Enter task"
       Text="{Binding Text}"
       Grid.Row="1"/>

<Button Text="Add"
        Command="{Binding AddCommand}"
        Grid.Row="1"
        Grid.Column="1"/>
...
```

### MainViewModel.cs

Adds the binded variables. When adding, check if the given text is empty, if empty stop else continue.

```cs
public MainViewModel()
    {
        Items = new ObservableCollection<string>();
    }

    [ObservableProperty]
    ObservableCollection<string> items;

    [ObservableProperty]
    string text;

    [RelayCommand]
    void Add()
    {
        if (string.IsNullOrEmpty(Text))
            return;

        Items.Add(Text);
        // add our item
        Text = string.Empty;
    }
    ...
```

## Removing Tasks

![image](https://user-images.githubusercontent.com/91055098/225263318-8b32ee98-3580-4467-bbf7-9eb811dfe52d.png)

![image](https://user-images.githubusercontent.com/91055098/225263365-de9d6cbf-839b-4296-ad1c-2469146af0b8.png)

![image](https://user-images.githubusercontent.com/91055098/225263415-57b3793c-a677-4afe-9b12-8929467293f7.png)

Remove a task by swiping a task to the left, where a delete button will appear. Click that button in which the task will be deleted.

### MainPage.xaml

Bind the delete button to trigger the Delete RelayCommand in the Viewmodel. Swiping animation is built in the library.

```xaml
...
<CollectionView.ItemTemplate>
    <DataTemplate x:DataType="{x:Type x:String}">
        <SwipeView>
            <SwipeView.RightItems>
                <SwipeItems>
                    <SwipeItem Text="Delete"
                               BackgroundColor="Red"
                               Command="{Binding Source={RelativeSource AncestorType={x:Type viewmodel:MainViewModel}}, Path=DeleteCommand}"
                               CommandParameter="{Binding .}"/>
                </SwipeItems>
            </SwipeView.RightItems>
            <Grid Padding="0,5">
                <Frame>
                    <Label Text="{Binding .}"
                           FontSize="24"/>
                </Frame>
            </Grid>
        </SwipeView>
    </DataTemplate>

</CollectionView.ItemTemplate>
...
```

### MainViewModel.cs

Deletes the task that contained the text content.

```cs
...
[RelayCommand]
void Delete(string s)
{
    if(Items.Contains(s))
    {
        Items.Remove(s);
    }
}
...
```
