## Pages

It all starts with the `#pages` block that is positioned in the center of the screen so that left, right, and top margins are equal. It is the main container and it may have one or many `.page` blocks. Page will have a frame of a specific color if its `style` attribute includes `border-color` property. Pages will reduce their width and padding relative to the browser viewport width.

``` html
<div id="pages">
  <div class="page" style="border-color: midnightblue;">
    <!-- Content -->
  </div>
</div>
```

### Content

Content can be structured using the `.grid`, which is based on the [16 column grid implementation](https://cdn.jsdelivr.net/npm/semantic-ui@2.4.2/dist/components/grid.css) from the excellent [Semantic UI](https://semantic-ui.com) framework. **Grid** consists of `.row`s and `.column`s, as you'd expect. **Rows** are groups of columns which are aligned horizontally. Rows can either be _explicit_, marked with an additional `.row` element, or _implicit_, automatically occurring when no more space is left in a previous row. **Columns** divide horizontal space into indivisible units. All columns in a grid must specify their width as proportion of the total available row width, e.g. `.four.wide`, `.eleven.wide`, etc.

`.stackable` grids will stack their columns on top of each other on mobile devices. `.divider` is useful to visually separate stacked columns. `.mobile.only` grids, rows, or columns would be visible only on mobile devices. `.center.aligned` grids, rows, or columns will attempt to center their content. There're no `.left.aligned` or `.right.aligned` classes, because everything is aligned to the left by default, and aligning to the right is almost never a good idea.  Grids can be nested.

``` html
<div class="stackable grid">
  <div class="row">
    <div class="three wide center aligned column">
      <!-- Avatar -->
    </div>
    <div class="thirteen wide column">
      <div class="stackable grid">
        <div class="sixteen wide column">
          <!-- Name, job, contacts -->
        </div>
        <div class="sixteen wide mobile only column">
          <div class="divider"></div>
        </div>
        <div class="sixteen wide column">
          <!-- Intro -->
        </div>
      </div>
    </div>
  </div>
  <div class="row">
    <div class="sixteen wide column">
      <div class="fat divider"></div>
    </div>
  </div>
  <div class="row">
    <div class="ten wide column">
      <!-- Experience -->
    </div>
    <div class="six wide column">
      <!-- Skills, character, education -->
    </div>
  </div>
</div>
```

## Scripts