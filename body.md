It all starts with the `#pages` block that is positioned in the center of the screen so that left, right, and top margins are equal. It is the main container and it may have one or many `.page` blocks. Page may have a `style="border-color: midnightblue;"`, `.grid` may define a layout of the page. Grid was based on the [grid implementation](https://cdn.jsdelivr.net/npm/semantic-ui@2.4.2/dist/components/grid.css) in the excellent [Semantic UI](https://semantic-ui.com) framework.

``` html
<div id="pages">
  <div class="page">
    <div class="stackable grid">
      <div class="row">
        <div class="three wide center aligned column">
          <!-- Avatar -->
        </div>
        <div class="thirteen wide column">
          <div class="stackable grid">
            <div class="sixteen wide column">
              <!-- Name, contacts -->
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
  </div>
</div>
```