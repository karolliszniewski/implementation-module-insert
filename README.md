### 1. First, create necessary directories in your theme:

```
app/design/frontend/YourVendor/YourTheme/Magento_Catalog/layout/
app/design/frontend/YourVendor/YourTheme/MageWorx_Downloads/templates/
```

```xml
// app/design/frontend/YourVendor/YourTheme/Magento_Catalog/layout/catalog_product_view.xml

<?xml version="1.0"?>
<page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">
    <body>
        <referenceContainer name="product.info.main">
            <block class="MageWorx\Downloads\Block\Catalog\Product\Attachments" 
                   name="product.attachments" 
                   template="MageWorx_Downloads::custom/attachment_container.phtml" 
                   after="product.info.price"/>
        </referenceContainer>
    </body>
</page>
```

### 2. Create custom template file:

```phtml
// app/design/frontend/YourVendor/YourTheme/MageWorx_Downloads/templates/custom/attachment_container.phtml

<?php
/**
 * @var \MageWorx\Downloads\Block\Catalog\Product\Attachments $block
 */
?>

<?php
$attachments = $block->getAttachments();
$blockTitle = $block->getBlockTitle();
$isGroupBySection = $block->isGroupBySection();
?>

<?php if ($attachments && count($attachments)): ?>
    <div class="product-attachments-container">
        <?php if ($blockTitle): ?>
            <div class="block-title">
                <strong><?= $block->escapeHtml($blockTitle) ?></strong>
            </div>
        <?php endif; ?>

        <div class="attachments-list">
            <?php if ($isGroupBySection): ?>
                <?php foreach ($block->getAttachmentsBySection() as $sectionId => $sectionData): ?>
                    <div class="section-container">
                        <div class="section-title">
                            <?= $block->escapeHtml($sectionData['name']) ?>
                        </div>
                        <?php foreach ($sectionData['attachments'] as $attachment): ?>
                            <div class="attachment-item">
                                <a href="<?= $block->escapeUrl($attachment->getDownloadUrl()) ?>" 
                                   class="attachment-link">
                                    <span class="attachment-name">
                                        <?= $block->escapeHtml($attachment->getName()) ?>
                                    </span>
                                    <?php if ($attachment->getSize()): ?>
                                        <span class="attachment-size">
                                            (<?= $block->escapeHtml($attachment->getSizeStr()) ?>)
                                        </span>
                                    <?php endif; ?>
                                </a>
                                <?php if ($attachment->getDescription()): ?>
                                    <div class="attachment-description">
                                        <?= $block->escapeHtml($attachment->getDescription()) ?>
                                    </div>
                                <?php endif; ?>
                            </div>
                        <?php endforeach; ?>
                    </div>
                <?php endforeach; ?>
            <?php else: ?>
                <?php foreach ($attachments as $attachment): ?>
                    <div class="attachment-item">
                        <a href="<?= $block->escapeUrl($attachment->getDownloadUrl()) ?>" 
                           class="attachment-link">
                            <span class="attachment-name">
                                <?= $block->escapeHtml($attachment->getName()) ?>
                            </span>
                            <?php if ($attachment->getSize()): ?>
                                <span class="attachment-size">
                                    (<?= $block->escapeHtml($attachment->getSizeStr()) ?>)
                                </span>
                            <?php endif; ?>
                        </a>
                        <?php if ($attachment->getDescription()): ?>
                            <div class="attachment-description">
                                <?= $block->escapeHtml($attachment->getDescription()) ?>
                            </div>
                        <?php endif; ?>
                    </div>
                <?php endforeach; ?>
            <?php endif; ?>
        </div>
    </div>
<?php endif; ?>
```



## Implementatio 2

```xml
<?xml version="1.0"?>
<page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">
    <body>
        <referenceBlock name="product.info.addto">
            <block class="Magento\Widget\Block\BlockInterface" name="custom.widget.instance">
                <arguments>
                    <argument name="widget_instance_id" xsi:type="number">18</argument>
                </arguments>
            </block>
        </referenceBlock>
    </body>
</page>
```


```phtml
<?php
$instanceId = 18; // ID Twojego widgetu

/** @var \Magento\Framework\View\LayoutInterface $layout */
$layout = $this->getLayout();

$widgetHtml = $layout->createBlock(\Magento\Widget\Block\BlockInterface::class)
    ->setWidgetInstanceId($instanceId)
    ->toHtml();

echo $widgetHtml;
?>
```
