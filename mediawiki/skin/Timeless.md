```
root@VM-0-12-ubuntu:/var/www/html/skins/Timeless/includes# vi TimelessTemplate.php
```

```php

        /**
         * Outputs the entire contents of the page
         */
        public function execute() {
                $this->sidebar = $this->getSidebar();

                // WikiBase sidebar thing
                if ( isset( $this->sidebar['wikibase-otherprojects'] ) ) {
                        $this->otherProjects = $this->sidebar['wikibase-otherprojects'];
                        unset( $this->sidebar['wikibase-otherprojects'] );
                }
                // Collection sidebar thing
                if ( isset( $this->sidebar['coll-print_export'] ) ) {
                        $this->collectionPortlet = $this->sidebar['coll-print_export'];
                        unset( $this->sidebar['coll-print_export'] );
                }

                $this->pileOfTools = $this->getPageTools();
                $userLinks = $this->getUserLinks();

                // Open html, body elements, etc
                $html = $this->get( 'headelement' );

                $html .= Html::openElement( 'div', [ 'id' => 'mw-wrapper', 'class' => $userLinks['class'] ] );

                $html .= Html::rawElement( 'div', [ 'id' => 'mw-header-container', 'class' => 'ts-container' ],
                        Html::rawElement( 'div', [ 'id' => 'mw-header', 'class' => 'ts-inner' ],
                                $userLinks['html'] .
                                $this->getLogo( 'p-logo-text', 'text' ) .
                                $this->getSearch()
                        ) .
                        $this->getClear()
                );
                $html .= $this->getHeaderHack();

                // For mobile
                $html .= Html::element( 'div', [ 'id' => 'menus-cover' ] );

                $html .= Html::rawElement( 'div', [ 'id' => 'mw-content-container', 'class' => 'ts-container' ],
                        Html::rawElement( 'div', [ 'id' => 'mw-content-block', 'class' => 'ts-inner' ],
                                Html::rawElement( 'div', [ 'id' => 'mw-content-wrapper' ],
                                        $this->getContentBlock() .
                                        $this->getAfterContent()
                                ) .
                                Html::rawElement( 'div', [ 'id' => 'mw-site-navigation' ],
                                        $this->getLogo( 'p-logo', 'image' ) .
                                        $this->getMainNavigation() .
                                        $this->getSidebarChunk(
                                                'site-tools',
                                                'timeless-sitetools',
                                                $this->getPortlet(
                                                        'tb',
                                                        $this->pileOfTools['general'],
                                                        'timeless-sitetools'
                                                )
                                        )
                                ) .
                                Html::rawElement( 'div', [ 'id' => 'mw-related-navigation' ],
                                        $this->getPageToolSidebar() .
                                        $this->getInterwikiLinks() .
                                        $this->getCategories()
                                ) .
                                $this->getClear()
                        )
                );

                $html .= Html::rawElement( 'div', [ 'id' => 'mw-footer-container', 'class' => 'ts-container' ],
                        Html::rawElement( 'div', [ 'id' => 'mw-footer', 'class' => 'ts-inner' ],
                                $this->getFooter()
                        )
                );

                $html .= Html::closeElement( 'div' );

                // BaseTemplate::printTrail() stuff (has no get version)
                // Required for RL to run
                $html .= MWDebug::getDebugHTML( $this->getSkin()->getContext() );
                $html .= $this->get( 'bottomscripts' );
                $html .= $this->get( 'reporttime' );

                $html .= Html::closeElement( 'body' );
                $html .= Html::closeElement( 'html' );

                // The unholy echo
                echo $html;
        }
```

```php

        /**
         * Generate the page content block
         * Broken out here due to the excessive indenting, or stuff.
         *
         * @return string html
         */
        protected function getContentBlock() {
                $html = Html::rawElement(
                        'div',
                        [ 'id' => 'content', 'class' => 'mw-body',  'role' => 'main' ],
                        $this->getSiteNotices() .
                        $this->getIndicators() .
                        $this->getToc() .
                        $this->getToc() .
                        $this->getToc() .
                        Html::rawElement(
                                'h1',
                                [
                                        'id' => 'firstHeading',
                                        'class' => 'firstHeading',
                                        'lang' => $this->get( 'pageLanguage' )
                                ],
                                $this->get( 'title' )
                        ) .
                        Html::rawElement(
                                'h1',
                                [
                                        'id' => 'firstHeading',
                                        'class' => 'firstHeading',
                                        'lang' => $this->get( 'pageLanguage' )
                                ],
                                $this->get( 'title' )
                        ) .
                        Html::rawElement( 'div', [ 'id' => 'bodyContentOuter' ],
                                Html::rawElement( 'div', [ 'id' => 'siteSub' ], $this->getMsg( 'tagline' )->parse() ) .
                                Html::rawElement( 'div', [ 'id' => 'siteSub' ], $this->getMsg( 'tagline' )->parse() ) .
                                Html::rawElement( 'div', [ 'id' => 'siteSub' ], $this->getMsg( 'tagline' )->parse() ) .
                                Html::rawElement( 'div', [ 'id' => 'siteSub' ], $this->getMsg( 'tagline' )->parse() ) .
                                Html::rawElement( 'div', [ 'id' => 'mw-page-header-links' ],
                                        $this->getPortlet(
                                                'namespaces',
                                                $this->pileOfTools['namespaces'],
                                                'timeless-namespaces',
                                                [ 'extra-classes' => 'tools-inline' ]
                                        ) .
                                        $this->getPortlet(
                                                'namespaces',
                                                $this->pileOfTools['namespaces'],
                                                'timeless-namespaces',
                                                [ 'extra-classes' => 'tools-inline' ]
                                        ) .
                                        $this->getPortlet(
                                                'more',
                                                $this->pileOfTools['more'],
                                                'timeless-more',
                                                [ 'extra-classes' => 'tools-inline' ]
                                        ) .
                                        $this->getVariants() .
                                        $this->getPortlet(
                                                'views',
                                                $this->pileOfTools['page-primary'],
                                                'timeless-pagetools',
                                                [ 'extra-classes' => 'tools-inline' ]
                                        )
                                ) .
                                $this->getClear() .
                                Html::rawElement( 'div', [ 'class' => 'mw-body-content', 'id' => 'bodyContent' ],
                                        $this->getContentSub() .
                                        $this->get( 'bodytext' ) .
                                        $this->get( 'bodytext' ) .
                                        $this->getClear()
                                )
                        )
                );

                return Html::rawElement( 'div', [ 'id' => 'mw-content' ], $html );
        }
```
